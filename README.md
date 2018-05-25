# Synology

[![Build Status](https://travis-ci.org/satreix/synology.svg)](https://travis-ci.org/satreix/synology) [![Requirements Status](https://requires.io/github/satreix/synology/requirements.svg?branch=master)](https://requires.io/github/satreix/synology/requirements/?branch=master)

Python3 binding to Synology DSM API. I refer to the following document:
[Synology_File_Station_API_Guide.pdf](https://global.download.synology.com/download/Document/DeveloperGuide/Synology_File_Station_API_Guide.pdf).
Any help is welcome, please fork this repo and make a pull request, or contact
me directly.

## API coverage

### Implemented

| Endpoint                         | Description                                                                                                                                                                                          |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SYNO.FileStation.Info            | Provide File Station info                                                                                                                                                                            |
| SYNO.FileStation.List            | List all shared folders, enumerate files in a shared folder,and get detailed file information                                                                                                        |
| SYNO.FileStation.DirSize         | Get the total size of files/folders within folder(s)                                                                                                                                                 |
| SYNO.FileStation.MD5             | Get MD5 of a file                                                                                                                                                                                    |
| SYNO.FileStation.CreateFolder    | Create folder(s)                                                                                                                                                                                     |
| SYNO.FileStation.Rename          | Rename a file/folder                                                                                                                                                                                 |
| SYNO.FileStation.Delete          | Delete files/folders                                                                                                                                                                                 |
| SYNO.FileStation.Search          | Search files on given criteria                                                                                                                                                                       |
| SYNO.FileStation.Thumb           | Get a thumbnail of a file                                                                                                                                                                            |
| SYNO.FileStation.CheckPermission | Check if the file/folder has a permission of a file/folder or not                                                                                                                                    |
| SYNO.FileStation.Upload          | Upload a file                                                                                                                                                                                        |
| SYNO.FileStation.Download        | Download files/folders                                                                                                                                                                               |
| SYNO.FileStation.Extract         | Extract an archive and do operations on an archive                                                                                                                                                   |
| SYNO.FileStation.BackgroundTask  | Get information regarding tasks of file operations which are run as the background process including copy, move, delete, compress and extract tasks or perform operations on these background tasks. |

### TODO

| Endpoint                         | Description                                                                                                                                                                                          |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SYNO.FileStation.VirtualFolder   | List all mount point folders of virtual file system, ex: CIFS or ISO                                                                                                                                 |
| SYNO.FileStation.Favorite        | Add a folder to user’s favorites or do operations on user’s favorites                                                                                                                                |
| SYNO.FileStation.Sharing         | Generate a sharing link to share files/folders with other people and perform operations on sharing links                                                                                             |
| SYNO.FileStation.CopyMove        | Copy/Move files/folders                                                                                                                                                                              |
| SYNO.FileStation.Compress        | Compress files/folders                                                                                                                                                                               |

## Install

```bash
pip install [--upgrade] https://github.com/satreix/synology/tarball/master#egg=synology
```

I do need to publish a PyPi version.

## Usage
```python
import config

from synology.api import Api
from synology.filestation import FileStation
from synology.utils import jsonprint

logging.basicConfig(level=logging.INFO)

# Instanciate directly
syno = Api(config.host, config.user, config.passwd)
syno.req('SYNO.API.Info', query='all')
syno.req('SYNO.FileStation.Info', end='info', extra='FileStation/',
    method='getinfo')
syno.jsonprint(syno.req(syno.endpoint('SYNO.FileStation.List',
    cgi='FileStation/file_share.cgi', method='list_share')))
syno.jsonprint(syno.req(syno.endpoint('SYNO.FileStation.Info',
    cgi='FileStation/info.cgi', method='getinfo')))

# Instanciate a FileStation
filestation = FileStation(config.host, config.user, config.passwd)
filestation.jsonprint(filestation.get_info())
filestation.jsonprint(filestation.get_shares())
```

## Other implementations

The following projects I found on PyPI:

- https://pypi.python.org/pypi/python-synology/0.1.0 ([sources](https://github.com/StaticCube/python-synology/)) Python API for communication with Synology DSM
- https://pypi.python.org/pypi/ds-down/0.2.1 ([sources](https://github.com/wor/ds-down)) Synology Download Station url adder.
- https://pypi.python.org/pypi/synoacl/0.0.2 ([sources](https://github.com/zub2/synoacl)) Python module for manipulating access control lists on a Synology NAS
- https://pypi.python.org/pypi/syncli/0.0.3 Python CLI for Synology DSM.
- https://pypi.python.org/pypi/synolopy/0.1.2 ([sources](https://github.com/thavel/synolopy)) Synology Python API
- https://pypi.python.org/pypi/syno/0.0.3 ([sources](https://github.com/bobuk/syno)) Synology API wrapper with aiohttp/asyncio

I would like to merge some of them together into one library and multiple apps using it.
