
Modification in the Encoder
============================

Following files in the source files are modified for inetgrating CNN model into AV1. The version of AV1 encoder is "1.0.0-2231-g9666276"

The following files are changed to pass the file name to CNN model

==============
aom/src/aom_codec.h
==============

Add the pointer of **filename** to aom_codec_ctx. Ths structure is the member called encoder of the structure stream state.

.. code-block:: c


 typedef struct aom_codec_ctx {
  const char *filename;
  ...
 } aom_codec_ctx_t;
 

==============
apps/aomenc.c
==============


Pass filename to the member enocder of the stream. 

.. code-block:: c

   FOREACH_STREAM(stream, streams) { 
      stream->encoder.filename=input.filename;
      initialize_encoder(stream, &global); }



====================================
aom/internal/aom_codec_internal.h
====================================

Add new member **filename** to the structure **aom_codec_priv**. It is a member of **aom_codec_ctx**.

.. code-block:: c

  struct aom_codec_priv {
    const char *filename;
    ...
  };
  

========================
av1/encoder/encoder.h
========================
Add member **filename** to the structure **AV1_COMP**. Also, add an array member that stores the partition index from CNN model.
  
.. code-block:: c

  typedef struct AV1_COMP {
    const char *filename;
    ...
    int prediction[3][135][240];
    ...
  } AV1_COMP;


======================
av1/encoder/encoder.c
======================

In the function **av1_create_compressor**, add the code to set up memory space for the prediction array.

.. code-block:: c

    memset(cpi->prediction, 0, sizeof(cpi->prediction));

=====================
av1/av1_cx_iface.c
=====================

In the function **encoder_init**, pass the filename from **aom_codec_ctx** to **AV1_COMP** by adding the following code.

.. code-block:: c

  if (res == AOM_CODEC_OK) {
        ...
        priv->cpi->filename = ctx->filename;     
      }

===========================
av1/encoder/encode_frame.c
===========================

In the function **encode_frame_internal**, add the following code to call CNN model in python script. This code first calls CNN model for partition prediction in python, and then read the prediction result from cu_depth.txt into the array set up beforehand.  

.. code-block:: c

  struct aom_usec_timer time_python;
      aom_usec_timer_start(&time_python);
      
      char cmd[100];
      sprintf(cmd, "python3 video_to_cu_depth_all.py %s %d %d %d %d %d",cpi->filename,cm->width, cm->height, cm->base_qindex, 0, cm->current_frame.order_hint);
      printf("%s\n", cmd);
      system(cmd)==0;
    
      aom_usec_timer_mark(&time_python);
      uint64_t time_for_python = aom_usec_timer_elapsed(&time_python);
      printf("time_for_python=%d\n", time_for_python);
      
      struct aom_usec_timer time_r;
      aom_usec_timer_start(&time_r);
      
      FILE *in_file  = fopen("cu_depth.txt", "r"); 
    	  if (in_file == NULL) 
    			{	
    			printf("Error! Could not open file\n"); 
    			exit(-1); // must include stdlib.h 
    			} 
    	
    	int block_width = cm->width>>6;
    	int block_height = cm->height>>6;
    	int cols = block_width*64 < cm->width ? block_width+1 : block_width; 
    	int rows = block_height*64 < cm->height ? block_height+1 : block_height; 
        int blocksize =0;
       	
    	for (int row = 0; row < rows; row++){
    	  for (int col = 0; col < cols; col++){
    	
    	   fscanf(in_file,"%1d", &cpi->prediction[blocksize][row][col]);
    	  }
    		}
    	block_width = cm->width>>5;
    	block_height = cm->height>>5;
    	cols = block_width*32 < cm->width ? block_width+1 : block_width; 
    	rows = block_height*32 < cm->height ? block_height+1 : block_height; 
    	blocksize =1;
    		
    	for (int row = 0; row < rows; row++){
    	   for (int col = 0; col < cols; col++){
    		
    		 fscanf(in_file,"%1d", &cpi->prediction[blocksize][row][col]);
    		}
    		}
    	block_width = cm->width>>4;
    	block_height = cm->height>>4;
    	cols = block_width*16 < cm->width ? block_width+1 : block_width; 
    	rows = block_height*16 < cm->height ? block_height+1 : block_height; 
    	blocksize =2;
    		
    		for (int row = 0; row < rows; row++){
    		  for (int col = 0; col < cols; col++){
    		
    		   fscanf(in_file,"%1d", &cpi->prediction[blocksize][row][col]);
    		  }
    	  	}	
    	
       aom_usec_timer_mark(&time_r);
       uint64_t time_for_read = aom_usec_timer_elapsed(&time_r);
       printf("time_for_read=%d\n", time_for_read);
     
 
Again, in **encode_frame.c**, in the function **rd_pick_partition**, add the following code to replace the original algorithms. Since only 3 block sizes are considered, the condition is set as following code.

.. code-block:: c

  if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	   {
          int row, col, blocksize;        
          switch(bsize){
	      case BLOCK_64X64: blocksize=0;
	                     row = mi_row>>4;
	                     col = mi_col>>4;
                             break;
              case BLOCK_32X32: blocksize=1;
                             row = mi_row>>3;
	                     col = mi_col>>3;
                             break;
              case BLOCK_16X16: blocksize=2;
                             row = mi_row>>2;
	                     col = mi_col>>2;
                             break;
                        }
	   switch (cpi->prediction[blocksize][row][col])
		    {
	         case PARTITION_NONE: goto DO_PARTITION_NONE;				   
	         case PARTITION_HORZ: goto DO_PARTITION_HORZ;
   	         case PARTITION_VERT:  goto DO_PARTITION_VERT;
	   	 case PARTITION_SPLIT: goto DO_PARTITION_SPLIT;
      		 case PARTITION_HORZ_A: goto DO_PARTITION_HORZ_A;
	      	 case PARTITION_HORZ_B: goto DO_PARTITION_HORZ_B;
	      	 case PARTITION_VERT_A: goto DO_PARTITION_VERT_A;
	      	 case PARTITION_VERT_B: goto DO_PARTITION_VERT_B;
      		 case PARTITION_HORZ_4: goto DO_PARTITION_HORZ_4;
   	   	 case PARTITION_VERT_4: goto DO_PARTITION_VERT_4;
	  	   }
	 	   }
    ...
    
    DO_PARTITION_NONE:
    
    if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	   partition_none_allowed=1;
    
    // PARTITION_NONE
    ...
    if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	   goto ENDING;
	
	
    DO_PARTITION_SPLIT:
    
    if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	   do_square_split=1;
    
    // PARTITION_SPLIT
    ...
    if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
   	goto ENDING;
   
    DO_PARTITION_HORZ:
    
    if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	 {
		 partition_horz_allowed=1;
	    	 prune_horz=0;
	    	 do_rectangular_split=1;
	  }
     
     //PARTITION_HORZ
     ...
     if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
		   goto ENDING;
		   
     DO_PARTITION_VERT:
     
     if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	     {   
	       partition_vert_allowed=1;
     	       prune_vert=0;
	       do_rectangular_split=1;
	      }
       
      // PARTITION_VERT
      ...
      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	     goto ENDING;
      ...
      
      DO_PARTITION_HORZ_A:

     if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)	  
            {	
                partition_horz_allowed=1;
	      	horza_partition_allowed=1;	
	     }
      
      //PARTITION_HORZ_A:
      ...
      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	     goto ENDING;
	   
      DO_PARTITION_HORZ_B:
      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	     {  
	           partition_horz_allowed=1;
		   horzb_partition_allowed=1;
              }
      
      // PARTITION_HORZ_B
      ...
      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	     goto ENDING;
		
      DO_PARTITION_VERT_A:
      
      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
		    {  
		    verta_partition_allowed=1;
	       	    partition_vert_allowed=1;
		    }
      
      // PARTITION_VERT_A
	     ...
      
      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
      goto ENDING;
	   
      DO_PARTITION_VERT_B:
      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
		    {   
		        vertb_partition_allowed=1;
		        partition_vert_allowed=1;
		    }
      
      // PARTITION_VERT_B
      ...
      
      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	     goto ENDING;
      
      DO_PARTITION_HORZ_4:

      if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
         {   
           partition_horz4_allowed=1;
           do_rectangular_split=1;
         }
       
       // PARTITION_HORZ_4
       ...
       if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	      goto ENDING;
		  
       DO_PARTITION_VERT_4:
       
       if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
		  {
		        partition_vert4_allowed=1;
		        do_rectangular_split=1;
		   }
		   
        // PARTITION_VERT_4
        ...
        if (bsize == BLOCK_64X64 || bsize == BLOCK_32X32 || bsize == BLOCK_16X16)
	       goto ENDING;
       
        ...

        ENDING:


      



