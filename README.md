__Subject

Varnish Brotli Support

__Body

Hello Christopher,

In order to enable Brotli support for Varnish solution please follow below steps:

1. Install vmod_brotli on your varnish environment (you can use the terminal command i.e. "[sudo] apt-get install vmod-brotli" or "yum install vmod-brotli").
2. Add the lines from Brotli support.txt file to your main VCL file in order to enable it's functionalities.

Command from the file allows varnish to handle the Brotli compression and store them in cache. At the same time clients not supporting brotli compression will receive the transcoded GZIP response instead.

Best Regards,
Emil Panasiuk
Varnish Software
