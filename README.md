# dw [BETA]

**Description**:

The tool has been created to speed up manual malware hunting tasks. A simple example is already covered in **“Use cases”** but let’s describe it verbally.
Imagine following situation, you found an open directory on the Internet, which is full of malicious samples (Example below)

![Open directoryąć](http://regenerus.com/download/2k20.jpg)

Normally to download these files you would have to manually save them (or retrieve the page source code and pull the href elements with regex or so, and then download them with curl or wget). When the amount of files is bigger than few it becomes unmanageable and simple waste of time. 

The “dw” if instructed to do so, could crawl the site for all available href elements and reconstruct their full URLs properly. The list of retrieved URLs could be automatically downloaded; or you could stop at this stage and decide to modify the list etc. The downloaded files could be compressed (zipped) and eventually automatically submitted to Antivirus vendor.

**Features**:

Some of available functionalities:

* Input URL de-obfuscation (hxxp[:]//120.132.17[.]180:66/ becomes http://120.132.17.180:66/)
* Input URL de-duplication 
* Site crawling for href elements (Two different modes)
* Bulk file downloads
* Downloaded files de-duplication (by file hash)
* Folder content de-duplication 
* File compression (.zip, custom files count per archive)
* Antivirus vendor submission (Requires specific config/%vendonr_name%.vd file)
* Input URL infromation lookup like proxy category etc. (VT information soon)
* URL proxy re-categorization (Will be shortly moved to a config/%vendonr_name%.vd file)
* Upload of an execution report (stdout) to pastebin
 
**Use cases:**

<pre> dw.py -i urls.txt -z -gl --download </pre>

* Load and deobfuscate URLs from input file (url.txt) [-i < filer >]
* Retrieve all available links/hrefs from loaded URLs [-gl]
* Download all detected links/hrefs [--download]
* Compress downloaded files and save in archive/ folder (Default 9 files by zip archive) [-z]

<pre> dw.py -gl --submit -i urls.txt </pre>

* Additionally submits compressed archives to configured vendors [--submit enables -z automatically]

<pre> dw.py -dedup -z -i downloads/ </pre>

* Load all files from input folder (downloads/) [-i < folder >]
* Deduplicate files 
* Compress all unique files from input folder and saves them to archive/ folder [-z]

<pre> dw.py --submit -i downloads/ </pre>

* Process files from downloads/ [zip them when necessary]
* Processed/compressed files are saved into archive/ folder
* Submits files from archive/ folder to configured vendors [--submit enables -z automatically]

<pre> dw.py -i urls.txt -r </pre> 

<span style="color:#FF0000"> Use with caution!!! </span>

* Retrieve all available links/hrefs from loaded URLs (Recursively) 
* Automatically enables -gl mode 


**Command Line:**

<pre>
optional arguments:
  -h, --help            show this help message and exit

Script arguments:

  -i INPUT, --input INPUT
                        Load and deobfuscate URLs from input file, or load
                        files from given folder for further processing
  -d DOWNLOAD_FOLDER, --download-folder DOWNLOAD_FOLDER
                        Specify custom download folder location (Default:
                        downloads/
  -a ARCHIVE_FOLDER, --archive ARCHIVE_FOLDER
                        Specify custom archive folder location (Default:
                        'archive/')
  -o OUTPUT_DIRECTORY   Copy loaded/deduplicated files into specified output
                        directory (Applicable when -dd is used)
  -dd, --dedup          Deduplicate the input and downloaded files
  -v VERBOSE_LEVEL, --verbose VERBOSE_LEVEL
                        Set the logging level to one of following: INFO,
                        WARNING, ERROR or DEBUG (Default: WARNING)
  --download            Download loaded or crawled URLs
  -z, --zip             Compress all downloaded files, or files from input
                        folder (If not zipped already)
  --limit-archive-items MAX_FILE_COUNT_PER_ARCHIVE
                        Sets the limit of files per archive (Default: 9). [0 =
                        Unlimited]

Crawling arguments:

  -gl, --get-links      Retrieve all available links/hrefs from loaded URLs
  -rl, --recursive-hostonly
                        Enable recursive crawling (Applies to -gl), but crawl
                        for hrefs containing the same url host as input url
                        (Sets --recursion-depth 0 and enables -gl)
  -r, --recursive       Enable recursive crawling (Applies to -gl, enables
                        -gl)
  -rd RECURSION_DEPTH, --recursion-depth RECURSION_DEPTH
                        Max recursion depth level for -r option (Default: 20)

Networking arguments:

  --user-agent USER_AGENT
                        User-agent string, which would be used by -gl and
                        --download
  --debug-requests      Sends GET/POST requests via local proxy server
                        127.0.0.1:8080

submission arguments:

  --submit              Submit files to AV vendors (Enables -z by default)
  --submit-url          Submit loaded URLs to PROXY vendors...
  -ui, --url-info       Retrieve URL information from supported vendors for
                        all loaded input URLs.
  -uif, --url-info-force
                        Force url info lookup for every crawled URL (NOT
                        recommended)
  -sc SUBMISSION_COMMENTS, --submission-comments SUBMISSION_COMMENTS
                        Insert submission comments (Default: <archive_name>)
  --proxy-vendors PROXY_VENDORS
                        Comma separated list of PROXY vendors used for URL
                        category lookup and submission
  --email SUBMITTER_EMAIL
                        Specify the submitter's e-mail address
  --proxy-category NEW_PROXY_CATEGORY
                        Specify new proxy category (If not specified default
                        proxy category will be used)

pastebin arguments:

  --pastebin-api PASTEBIN_API_KEY
                        API dev key for pastebin.com (If not specified, other
                        pastebin params would be ignored)
  -pu, --pastebin-upload
                        Uploads stdout to pastebin and prints the paste's url
  -pv PASTEBIN_TYPE, --pastebin-visibility PASTEBIN_TYPE
                        Set the paste visibility: 0 - Public or 2 - Private
                        (Default: 0)
  -pe PASTEBIN_PASTE_EXPIRATION, --pastebin-expiration PASTEBIN_PASTE_EXPIRATION
                        Set the paste expiration time to one of following:
                        'N': 'Never', '10M': '10 Minutes','1H': '1 Hour','1D':
                        '1 Day','1W': '1 Week','2W': '2 Weeks','1M': '1 Month'
                        ... (Default: 1H)
  -pt PASTEBIN_TITLE, --pastebin-title PASTEBIN_TITLE
                        Paste title
</pre>
 
**Change log:**

Ver. 0.0.8:

* -r, --recursive: Enable recursive crawling
* -rd, --recursion-depth: Max recursion depth level (default: 20)
* Simplified get_hrefs() code
* get_hrefs will use the requests session object and mix of HEAD and GET requests to speed up crawling performance
* get_hrefs set to return unique hrefs only 

Ver. 0.0.9:

* Fixes to download and get_hrefs functions
* If verbose level is set to DEBUG, print the href every time it's added to links list 
* Documentation update 

Ver. 0.1.0:

* Fixes to get_hrefs function
* "-rl", "--recursive-hostonly". Would crawl webistes which have the same url host as the input URL (Recommended)
* Small error handling to showing archive content (Didn't work for .jar files)
* Fix to download function (it was corrupting files)
* Documentation update

Ver. 0.1.1:

* Fix to download function (it was corrupting files)
* Documentation update

Ver. 0.1.2:

* Fix to download function (Logic imrpoved)

Ver. 0.1.3:

* Supressed the warning: urllib3/connectionpool.py:852: InsecureRequestWarning: Unverified HTTPS request is being made

Ver. 0.1.4:

* Critical fix to get_hrefs (Automatic parent folder and mod_autoindex detection; preventing back loops).

Ver. 0.1.5:

* Detect and skip links automatically created in open directory like: Name, Last modified, Size, Description

Ver. 0.1.7:

* Slight logic change to tasks execution 
* New parameters for input and downloaded files deduplication and copying files to an output folder 
* BC proxy lookup support
* uniq class for handling deduplication 
* Submitter class (so far for proxy lookup only, but soon for VT as well)

Ver. 0.1.7:

* If DEBUG mode enabled, it would print detected href's mime type if such was directly sent by the server 
* Download function prints detailed info about downloaded file like:  file_hash,file_destination,file_mime_type,proxy_category,url 

Ver. 0.1.9:

* Params names and documentation update

Ver. 0.2.0:

* --url-info, --submit-url, --email < email_address >, used for querying current proxy category or submitting new one for loaded URLs 
* --download, would enable download action
* --skip-download got removed
* --proxy-category: Specify new proxy category (Default: 'Malicious Sources/Malnets')
* --proxy-vendors: Comma separated list of PROXY vendors used for URL category lookup and submission

Ver. 0.2.1:

* Crawling for a hrefs shall be faster (the tool would flag as final href if the resource has known file extensions)

Ver. 0.2.2:

* Print deduplicated links
* Minor fixes

Ver. 0.2.3:

* Cosmetic changes (working on adding smb support)

Ver. 0.2.4:

* The session would use the user-agent specified by the user via --user-agent, if such is not specified it would use the one pointed by current_user_agent_index variable

Ver. 0.2.5:

* Cosmetic changes (--submit is not disabling -gl nor -rl params)

Ver. 0.2.6:

* New params for pastebin upload (run -h to find out)
* Few cosmetic and printing code changes 
* Changed the display of -h param groups 
* No need to specify -i urls.txr (if urls.txt exist)
* Check_args function adjustment  

Ver. 0.2.7:

* FIX: _urls variable was not properly cleared, hence duplicating output HREFs
* FIX: If -gl, -rl, -r does not find any hrefs, the input URL will be added to HREFs list

Ver. 0.2.8:
* pastebin: 1W instead of 1H

* -i would skip entries from URL input file which starts with "#"
* --url-info prints proxy category, ip, doamin, and url 
* Code re-design to support URL object (which would hold all information about the URL)
* Update to file_extensions dictionary (i still need to add a lot!)

Ver. 0.2.9:
* Quick fix in url.py->parse_url() ... shall be parsing URLs correctly

Ver. 0.3.0:
* Update to file_extensions (it has 76 extensions of well known files)
* Removed parse_urls from dw (it's now part of url object)
* Cosmetic code changes to logging

Ver. 0.3.1:
* Code fix: Again for url.py -> parse_urls (i missed one error last time)

Ver. 0.3.2:
* Code update: to get_hrefs. Preventing a case with a continuous loop

Ver. 0.3.3:
* Fix to Bluecoat proxy category lookup and submission (URL changes, POST data changes etc.)
* Added actual check if captcha is required, if not, no need to download it from the server.

Ver. 0.3.4:
* Cosmetic code change in the function loading URLs from a file (it would skip the lines which are just '\n')
* --url-info-force is well respected by --url-info 

Ver. 0.3.5:
* url parsing improvements 
* preparing for other enhancements like smb support, submission tracking (databse)