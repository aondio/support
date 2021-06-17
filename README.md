__Subject

Varnish Brotli Support

__Body

Hello Christopher,

In order to enable Brotli support for Varnish solution please follow below steps:

1. Install vmod_brotli on your varnish environment (you can use the terminal command i.e. "[sudo] apt-get install vmod-brotli" or "yum install vmod-brotli")
2. Add the following lines to your main VCL file in order to enable it's functionalities:

vcl 4.1;
import brotli;
sub vcl_init {
 brotli.init(BOTH, transcode = true);
}
sub vcl_backend_response {
 if (beresp.http.content-encoding ~ "gzip" ||
 beresp.http.content-type ~ "text") {
 brotli.compress();
 }
}

Above command allows varnish to handle the Brotli compression and store them in cache. Clients not supporting brotli compression will receive the transcoded GZIP response instead.

Best Regards,
Emil Panasiuk
Varnish Software
