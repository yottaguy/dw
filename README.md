# dw (Downloader) [BETA]

**Description**:

The tool has been created to speed up manual malware hunting tasks. A simple example is already covered in **“Use cases”** but let’s describe it verbally.
Imagine following situation, you found an open directory on the Internet, which is full of malicious samples (Example below)

![Open directoryąć](http://regenerus.com/download/2k20.jpg)

Normally to download these files you would have to manually save them (or retrieve the page source code and pull the href elements with regex or so, and then download them with curl or wget). When the amount of files is bigger than few it becomes unmanageable and simple waste of time. 

The “dw” if instructed to do so, could crawl the site for all available href elements and reconstruct their full URLs properly. The list of retrieved URLs could be automatically downloaded; or you could stop at this stage and decide to modify the list etc. The downloaded files could be compressed (zipped) and eventually automatically submitted to Antivirus vendor.

The tool is quite granular: 
* You could decide to only crawl for href elements from given URL(s) or to directly download URLs indicated in the input file.
* You could only compress downloaded files 
* You could only submit files from given folder to Antivirus vendor


**Features**:

* Accepts basic URL obfuscation which gets automatically resolved
  <pre>
  urls.txt:
    hxxp://2k20[.]tk/
    http[:]//120.132.17[.]180:66/ </pre>
  <pre>
  Parsing URL: hxxp://2k20[.]tk/ to: http://2k20.tk/
  Parsing URL: http[:]//120.132.17[.]180:66/ to: http://120.132.17.180:66/
  </pre>
* Built-in links/hrefs detection
  <pre>Getting hrefs from: http://2k20.tk/
  http://2k20.tk//0199.doc
  http://2k20.tk//1.exe
  http://2k20.tk//1.hta
  http://2k20.tk//1.rar
  http://2k20.tk//1.zip
  http://2k20.tk//8570.docx
  http://2k20.tk//8759.doc
  http://2k20.tk//a.apk
  http://2k20.tk//doc.doc</pre>
* Bulk file downloads 
  <pre>URL Download -> SUCCESS -> [HTTP200] - URL: http://2k20.tk//0199.doc
  [sha256: fe48b06516bf8939fe6b72808520435a98ec29fcbff9a324842c14abb10ec489] - downloads//0199.doc
  URL Download -> SUCCESS -> [HTTP200] - URL: http://2k20.tk//1.exe
  [sha256: 7b873da42a24ef30d6f523411f40c593a401ebfc9461cc3d93058c8ab8659225] - downloads//1.exe
  URL Download -> SUCCESS -> [HTTP200] - URL: http://2k20.tk//1.hta
  [sha256: b8397dac9b00dabcc65e0bf0505c74a134d570674829e901cf10bd4a047db09f] - downloads//1.hta
  URL Download -> SUCCESS -> [HTTP200] - URL: http://2k20.tk//1.rar
  [sha256: 9ac47bd4e34cc77a2abc3eb7d62dbb246312c748f4e39cd5351cc84022878424] - downloads//1.rar</pre>
* Automatic compression
  <pre>Add 'downloads//0199.doc' to: 'archive/samples-1.zip'
  Add 'downloads//1.exe' to: 'archive/samples-1.zip'
  Add 'downloads//1.hta' to: 'archive/samples-1.zip'
  Add 'downloads//1.rar' to: 'archive/samples-1.zip'</pre>
* Vendor submission (Requires specific config/%vendonr_name%.vd file)
  <pre>śsubmit - Submitting: archive/samples-2.zip to: https://...
  submit - Submission OK -> archive/samples-2.zip</pre>
* Recursive < a href > crawling (option -r): 
  <pre> dw.py -i urls.txt -gl --skip-download -r
    Getting hrefs from: http://109.234.36.233/bot
    All retrieved HREFs:
    http://109.234.36.233/bot/
    http://109.234.36.233/bot/.vs/
    http://109.234.36.233/bot/.vs/LoaderBot/
    ...
    http://109.234.36.233/bot/Miner/bin/Release/LoaderBot.vshost.exe
    http://109.234.36.233/bot/Miner/bin/Release/LoaderBot.vshost.exe.config
    http://109.234.36.233/bot/Miner/bin/Release/LoaderBot.vshost.exe.manifest
    ...
  </pre>  

**Use cases:**

<pre> dw.py -z -gl -i urls.txt </pre>

* Load and deobfuscate URLs from input file (url.txt) [-i < filer >]
* Retrieve all available links/hrefs from loaded URLs [-gl]
* Download all detected links/hrefs [If --skip-download not specified]
* Compress downloaded files and save in archive/ folder (Default 9 files by zip archive) [-z]

<pre> dw.py -gl --submit -i urls.txt </pre>

* Additionally submits compressed archives to configured vendors [--submit enables -z automatically]

<pre> dw.py -z -i downloads/ </pre>

* Load all files from input folder (downloads/) [-i < folder >]
* Compress all files from input folder and saves them to archive/ folder [-z]

<pre> dw.py --submit -i downloads/ </pre>

* Process files from downloads/ [zip them when necessary]
* Processed/compressed files are saved into archive/ folder
* Submits files from archive/ folder to configured vendors [--submit enables -z automatically]

**Change log:**

Ver. 0.0.8:

* -r, --recursive: Enable recursive crawling
* -rd, --recursion-depth: Max recursion depth level (default: 20)