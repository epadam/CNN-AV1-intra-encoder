
Modification in the Encoder
=======

Following files in the source files are modified for inetgrating CNN model into AV1.

The following files are changed to pass the file name to CNN model


In **aom_codec.h**, add the pointer of **filename** to aom_codec_ctx. Ths structure is the member called encoder of the structure stream state.

.. code-block:: c


 typedef struct aom_codec_ctx {
  const char *filename;
  const char *name;             /**< Printable interface name */
  aom_codec_iface_t *iface;     /**< Interface pointers */
  aom_codec_err_t err;          /**< Last returned error */
  const char *err_detail;       /**< Detailed info, if available */
  aom_codec_flags_t init_flags; /**< Flags passed at init time */
  union {
    /**< Decoder Configuration Pointer */
    const struct aom_codec_dec_cfg *dec;
    /**< Encoder Configuration Pointer */
    const struct aom_codec_enc_cfg *enc;
    const void *raw;
  } config;               /**< Configuration pointer aliasing union */
  aom_codec_priv_t *priv; /**< Algorithm private storage */
 } aom_codec_ctx_t;
 



In **aomenc.c**, pass filename to the member enocder of the stream. 

.. code-block:: c

   FOREACH_STREAM(stream, streams) { 
      stream->encoder.filename=input.filename;
      initialize_encoder(stream, &global); }

In **aom_codec_internal.h**, again, add new member **filename** to the structure **aom_codec_priv**. It is a member of **aom_codec_ctx**.

.. code-block:: c

  struct aom_codec_priv {
    const char *filename;
    const char *err_detail;
    aom_codec_flags_t init_flags;
    struct {
      aom_codec_priv_cb_pair_t put_frame_cb;
      aom_codec_priv_cb_pair_t put_slice_cb;
    } dec;
    struct {
      aom_fixed_buf_t cx_data_dst_buf;
      unsigned int cx_data_pad_before;
      unsigned int cx_data_pad_after;
      aom_codec_cx_pkt_t cx_data_pkt;
      unsigned int total_encoders;
    } enc;
  };
  
  
  In **encoder.h**, add member **filename** to the structure **AV1_COMP**. Also, add an array member that stores the partition index from CNN model.
  
  
.. code-block:: c

  typedef struct AV1_COMP {
    const char *filename;
    ...
    int prediction[3][135][240];
  } AV1_COMP;
  
In **encoder.c**, in the function **av1_create_compressor**, add the code to set up memory space for the prediction array.

.. code-block:: c

    memset(cpi->prediction, 0, sizeof(cpi->prediction));


  
Then in **av1_cx_iface.c**, in the function **encoder_init**, pass the filename from **aom_codec_ctx** to **AV1_COMP** by adding the following code.

.. code-block:: c

  if (res == AOM_CODEC_OK) {
        ...
        priv->cpi->filename = ctx->filename;     
      }


In **encode_frame.c**, in the function **encode_frame_internal**, add the following code to call CNN model in python script. This code first calls CNN model for partition prediction in python, and then read the prediction result from cu_depth.txt into the array set up beforehand.  

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
     
 


  



