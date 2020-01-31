# Lucky Money AR Application Workshop

Welcome to Lucky Money AR Application Workshop. 

At [Lunar New Year](https://en.wikipedia.org/wiki/Chinese_New_Year), it’s a tradition to give [Red Packets](https://en.wikipedia.org/wiki/Red_envelope) as the gifts to your friends and family. There are filled with lucky money and is a symbol of blessings for good luck in the new year. As Internet has developed fast in the past few years, nowadays, comparing with cash, Chinese people prefer sending virtual red packets over the Internet during the Lunar New Year. Tens of billions of red packets are sent via this way every year.

In this workshop, you will learn how to add an Argumented Reality(AR) Red Packet feature to your application using **Amazon Sumerian**, **AWS Amplify**, **Amazon Cognito**, **AWS AppSync**, **Amazon DynamoDB** and etc.

Visit [https://lucky-money.lab798.com](https://lucky-money.lab798.com/) to getting started with the workshop.

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
`git clone git@github.com:aws-samples/lucky-money-ar-workshop.git` (or your fork)

#### Clone the theme submodule:

```shell script
cd lucky-money-ar-workshop
git submodule init
git submodule update
```

#### Run Hugo locally:
Run `hugo server -D`, and open `http://localhost:1313/` in your browser.
or

Run `hugo` will build your content locally and output to `./public/`

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

## Contributor

[@JoeShi](https://github.com/joeshi/), 
[@go4real](https://github.com/go4real/),
[@fanyizhe](https://github.com/fanyizhe/), 
[@salander0411](https://github.com/salander0411/), 
[@yuan00yuan](https://github.com/yuan00yuan/)

