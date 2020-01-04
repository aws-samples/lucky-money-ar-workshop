# AKO2020 - Lucky Money AR Application Workshop

Welcome AKO2020 Lucky Money AR Application Workshp


## Setup:

The workshop website is built on [Hugo](https://gohugo.io/). You do **NOT** need to 
install Hugo and clone this repo if you want to run this workshop. The following step 
is **ONLY** used to build the workshop material itself and run a localhost version of the content. 

#### Install Hugo:
On a mac:

`brew install hugo`

On Linux:
  - Download from the releases page: https://github.com/gohugoio/hugo/releases/tag/v0.59.0
  - Extract and save the executable to `/usr/local/bin`

#### Clone this repo:
From wherever you checkout repos:
`git clone git@github.com:joeshi/ako2020-lucky-money.git` (or your fork)

#### Clone the theme submodule:

```shell script
cd ako2020-lucky-money
git submodule init
git submodule update
```

#### Run Hugo locally:
Run `hugo server -D`, and open `http://localhost:1313/` in your browser.
or

Run `hugo` will build your content locally and output to `./public/`

## License Summary

This sample code is made available under the MIT-0 license. See the LICENSE file.



