sami-parser
===========

JavaScript SAMI(Synchronized Accessible Media Interchange) parser for nodejs.

[![NPM](https://nodei.co/npm/sami-parser.png?downloads=true)](https://nodei.co/npm/sami-parser/)

## Install

	npm install sami-parser

## Example
sami:

	<SAMI>
	<HEAD>
	<TITLE>Lorem ipsum</TITLE>
	<STYLE TYPE="text/css">
	<!--
	P { margin-left:2pt; margin-right:2pt; margin-bottom:1pt;
	    margin-top:1pt; font-size:12pt; text-align:center;
	    font-family:굴림, 굴림; font-weight:normal; color:white;
	    }
	.KRCC { Name:한국어; lang:ko-KR; SAMIType:CC; }
	#STDPrn { Name:Standard Print; }
	#LargePrn { Name:Large Print (26pt); font-size:26pt; }
	#SmallPrn { Name:Small Print (14pt); font-size:14pt; }
	-->
	</STYLE>
	</HEAD>
	<BODY>
	<SYNC Start=6144><P Class=KRCC>
	<font face=돋움>Lorem ipsum dolor sit amet, consectetur<br><b>Lorem ipsum dolor sit amet, consectetur</b><br>
	<font size=2>Lorem ipsum dolor sit amet, consectetur
	<SYNC Start=10102><P Class=KRCC>&nbsp;
	<SYNC Start=10122 ><P Class=ENCC>&nbsp;
	<SYNC Start=10142 ><P Class=ENCC>&nbsp;
	<SYNC Start=10162 ><P Class=ENCC>&nbsp;
	<SYNC Start=17976><P Class=KRCC>
	<font face=돋움>Lorem ipsum dolor sit amet, consectetur<br><b>Lorem ipsum dolor sit amet, consec
	tetur</b><br><font size=2>Lorem ipsum dolor sit amet, consectetur
	<SYNC Start
	=  7007908 ><P Class=ENCC>&nbsp;
	<SYNC Start
	=  7007918 ><P Class=ENCC>&nbsp;
	<SYNC Start
	=  7007920 ><P Class=ENCC>&nbsp;
	</BODY>
	</SAMI>

parse tree: 

	{
	  "result": [
	    {
	      "startTime": 6144,
	      "languages": {
	        "ko": "Lorem ipsum dolor sit amet, consectetur\nLorem ipsum dolor sit amet, consectetur\nLorem ipsum dolor sit amet, consectetur"
	      },
	      "endTime": 10102
	    },
	    {
	      "startTime": 17976,
	      "languages": {
	        "ko": "Lorem ipsum dolor sit amet, consectetur\nLorem ipsum dolor sit amet, consectetur\nLorem ipsum dolor sit amet, consectetur"
	      },
	      "endTime": 7007908
	    }
	  ],
	  "errors": []
	}

## Usage

	parser = require('sami-parser')
	parser.parse('<sync start="123">Lorem ipsum</sync>', options = {})
	// or
	parser.parseFile('lorem.smi', options = {})

## Options

* `definedLangs` - pre-defined language object. Will be used when unable to recognize language code. Default is `{}`.
* `duration` - default duration between startTime and endTime. Default `10000`(10000ms; 10 seconds).

example: 

    definedLangs: {
	    KRCC: { # class name in <sync> ex) <SYNC start="123"><P class="KRCC">
		  lang: 'ko'	# ISO639
	      reClassName: new RegExp("class[^=]*?=[\"'\S]*(KRCC)['\"\S]?", 'i') # Regular Expression to match the class name.
	    }
	},
	duration: 10000

## License
MIT
