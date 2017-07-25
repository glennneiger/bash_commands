# bash_commands

Enabling Push Status Messages on OS X

To obtain the current state of the APNs daemon on OS X, use this command:

$ /System/Library/PrivateFrameworks/ApplePushService.framework/apsctl status

To enable logging on OS X, use the following commands:

$ sudo touch /Library/Logs/apsd.log
$ sudo defaults write /Library/Preferences/com.apple.apsd APSWriteLogs -bool TRUE
$ sudo defaults write /Library/Preferences/com.apple.apsd APSLogLevel -int 7
$ sudo killall apsd
The logs are stored in /Library/Logs/apsd.log


Issues with Sending Push Notifications

If your app is not receiving push notifications, and everything looks correct on the device or computer, here are some things to check on the server side.

Problems Connecting to the Push Service
One possibility is that your server is unable to connect to the push service. This can mean that you don't have the certificate chain needed for TLS/SSL to validate the connection to the service. In addition to the SSL identity (certificate and associated private key) created by Member Center, you should also install the root certificate on your provider. This allows TLS/SSL to verify the full APNs server cert chain.

The root certificate depends on which APNs API you’re using. If you’re using the HTTP/2 APNs provider API, the root certificate is the GeoTrust Global CA root certificate. You can download this from GeoTrust’s site.

If you’re using the binary provider API, the root certificate is the Entrust.net Certification Authority (2048) root certificate. You can download it from Entrust's site.

Also verify that these identities are installed in the correct location for your provider and that your provider has permission to read them.

You can test the TLS/SSL handshake using the OpenSSL s_client command, like this for the HTTP/2 APNs provider API:

$ openssl s_client -connect api.development.push.apple.com:443 -cert YourSSLCertAndPrivateKey.pem -debug -showcerts -CAfile server-ca-cert.pem
Important: The HTTP/2 APNs provider API requires TLS 1.2. TLS 1.2 requires OpenSSL 1.0.1 or later. The version of OpenSSL 0.9.x that is shipped with OS X does not support TLS 1.2 or HTTP/2.
Or, for the binary provider API:

$ openssl s_client -connect gateway.sandbox.push.apple.com:2195 -cert YourSSLCertAndPrivateKey.pem -debug -showcerts -CAfile server-ca-cert.pem
where server-ca-cert.pem is the Entrust CA (2048) root certificate.

Be sure the SSL identity and the hostname are the correct ones for the push environment you're testing. You can configure your App ID in your developer account separately for the sandbox and production environment. If you don’t request a universal push certificate, you will be issued a separate identity for each environment.

Using the sandbox SSL identity to try to connect to the production environment will return an error like this:

CRITICAL | 14:48:40.304061 | Exception creating ssl connection to Apple: [Errno 1] _ssl.c:480: error:14094414:SSL routines:SSL3_READ_BYTES:sslv3 alert certificate revoked

Note: The exact error will depend on your particular operating environment.
One common OpenSSL error is verify error:num=20:unable to get local issuer certificate.

That message means that one or more of the certificates from the server could not be verified because the issuer (root or intermediate) certificates were not found.

The CAfile argument to s_client specifies the trusted root certificates to use to verify the server certificate. The trusted root certificate for the push servers is the GeoTrust or Entrust root certificate mentioned previously.

If you can't even open a connection to APNs, perhaps your APNs TLS/SSL certificate has expired. These certificates are valid for one year but production APNs certificates can be renewed at any time.

Another possibility is that you've connected too many times to APNs and further connections have been temporarily blocked. As is documented in the Local and Remote Notification Programming Guide, developers are expected to open a connection and leave it open. If a connection is opened and closed repeatedly, APNs will treat it as a denial of service attack and block connections for a period of time.

This temporary block will expire if no connection attempts are made for about one hour.

Yet another possibility is that there is a firewall blocking access to the ports used by APNs. Please see IP Address Range Used by the Push Service for details. Try running a telnet command on your server to see if the server can reach APNs, like this:

$ telnet 1-courier.push.apple.com 5223

$ telnet gateway.sandbox.push.apple.com 2195

$ telnet gateway.push.apple.com 2195


// all write file descriptors in use
```
sudo fs_usage |grep write |grep -Fv -e grep -e Chrom -e syslogd
```

// filter out stuff in an original file by the stuff in another file
```
awk 'FNR==NR{old[$0];next};!($0 in old)' libsystem.txt engage.txt > libsystem_removed.txt
```

// create a filter file
```
grep -e "libsystem_network.dylib" engage.txt > network.txt
```

// List all files by size greater than 1GB
```
find / -type f -size +1GB -exec ls -lh {} \; 2> /dev/null
```

// List all TCP connections at ipv4 at port 9001
```
lsof -n -i4TCP:9001
```

// tree a directory
```
#!/bin/bash

MAGIC='s;[^/]*/;|____;g;s;____|; |;g'

if [ "$#" -gt 0 ] ; then
   dirlist="$@"
else
   dirlist="."
fi

for x in $dirlist; do
     find "$x" -print | sed -e "$MAGIC"
done
```
