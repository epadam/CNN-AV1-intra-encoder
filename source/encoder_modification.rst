
Modification in the Encoder
=======

Following files in the source files are modified for inetgrating CNN model into AV1.

The following files are changed to pass the file name to 

aom_codec.h

::
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

