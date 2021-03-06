<a href="https://www.bernardi.cloud/">
    <img src=".readme-files/hugo-uploader-logo-72.png" alt="Hugo Uploader logo" title="Hugo Uploader" align="right" height="72" />
</a>

# Hugo Uploader
> Command line tool for Hugo-based websites FTP differential upload.

[![Python](https://img.shields.io/badge/python-v3.7+-blue.svg)](https://www.python.org)
[![License](https://img.shields.io/github/license/bernarpa/hugo-uploader.svg)](https://opensource.org/licenses/AGPL-3.0)
[![GitHub issues](https://img.shields.io/github/issues/bernarpa/hugo-uploader.svg)](https://github.com/bernarpa/hugo-uploader/issues)

## Table of contents

- [Why Hugo Uploader](#why-hugo-uploader)
- [Usage](#usage)
- [How does it work](#how-does-it-work)
- [ACHTUNG](#achtung)
- [License](#license)

## Why Hugo Uploader

Even though I love using the [Hugo static site generator](https://gohugo.io/) I've always found the process of uploading compiled websites via FTP rather clumsy.

I've tried both interactive graphical utilities, such as [FileZilla](https://filezilla-project.org/) and simple command line scripts based on [LFTP](http://lftp.yar.ru/) or [NcFTP](https://www.ncftp.com/), but somehow the results have always been suboptimal with respect to differential upload capabilities (uploading only new or modified files):
  
  - using timestamps to verify which files have changed remotely is often unreliable;
  - using file sizes got me a few headaches in the recent past, let's say that the probability of a modified page retaining the same size as before is greater than you might think.

Obviously these issues are caused by limitations of the FTP protocol, the aforementioned tools are great. However, for the specific scenario of static generated websites and FTP I needed a more precise differential uploader. Therefore, I took the occasion to play with Python a bit and here's Hugo Uploader.

## Usage

To generate and upload a Hugo-based website with Hugo Uploader, just run the program from the root directory of the website:

    $ cd /hugo/website
    $ hugo-uploader

Alternatively, you may specify the Hugo website root directory as a command line argument:

    $ hugo-uploader /hugo/website

## How does it work

Firstly, Hugo Uploader will invoke the `hugo` command to build a minified version on the website in the `public` directory.

Secondly, if Hugo Uploader was run for the first time, it will ask for FTP connection details. Such details will be stored in the `.hugo-uploader.cfg` file within the website root directory. Please note that this file will contain the cleartext FTP password as well, you might want to keep it away from privy eyes.

Finally, Hugo Uploader will upload via FTP every new or changed `public` file. If Hugo Uploader was executed for the first time it will simply upload each file, otherwise it will upload only new or modified items.

In order to find out which files have been added or modified, Hugo Uploader stores a `.hash-list` file on the website root directory. This file contains a list of all files in the `public` directory and their cryptographic hashes from the last successful FTP upload.

## ACHTUNG

Please note that storing `.hugo-uploader.cfg` and `.hash-list` on the website root directory works well for my personal workflow, as I keep my Hugo-based websites on private git repositories. Therefore:

  - `.hugo-uploader.cfg` will not be publicly exposed;
  - `.hash-list` is stored in the git repository as part of the website, together with the changes that I will perform from time to time before generating it, which I find rather elegant. :wink:

## License

Hugo Uploader is licensed under the terms of the GNU Affero General Public License version 3.
