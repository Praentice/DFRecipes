# Regex
The aim of this page is to give tips and tricks regarding regex and how you can write your own regular expressions for forensic purposes. Skip to the "Ready-to-use regex" if you're looking for useful regex.

## Charsets
Here's some examples to understand charsets :
### Basic

- `[abc]` : Match every occurences of a, b and c.
- `[a-c]` : The same as above
- `[a-cx-z]` : Match every occurences of a, b, c, x, y and z.
- `[a-zA-Z]` : Match any single lowercase or uppercase letter
- `[^k]` : Match any single letter except the letter *k* which is excluded from the charset
### Advanced

Once you understood complety the Basic part of charset, we can now go through advanced examples of using charset : 
- `[cfh]at` : Match the words cat, fat, hat but not bat for example
- `[Ff]ile[1-9]` : Match the words File1, File2, file3, file4, file5, File7, file9
- `[Ff]ile[^7]` : Same as above except that it doesn't match the word File7
I hope these examples helped you to understand how the charset capabilities can be combined to create regex rules.

## Wildcards and optional characters

- `a.c` : Match for example aac, abc, a0c, a$c and so on. Please note here that the `.` dot is a wildcard which allows us to match any single characters except the line break.
- `abc?` : Match for example ab, abc. When using the `?` question mark, any single character which precede it will be marked as optionnal.
- `\` : This special character can be used to escape any specials characters such as `.`, `?`...
For example, the regex `cat\.xyz` will only match the string cat.xyz and not cataxyz, catbxyz and so on.

## Metacharacters and repetitions
Even though charset are useful when it comes to creating regex/

### Metacharacters

- `\d` matches a digit, like `9`  
- `\D` matches a non-digit, like `A` or `@`  
- `\w` matches an alphanumeric character, like `a` or `3`  
- `\W` matches a non-alphanumeric character, like `!` or `#`  
- `\s` matches a whitespace character (spaces, tabs, and line breaks)  
- `\S` matches everything else (alphanumeric characters and symbols)

### Repetitions

- `{12}` - **exactly 12** times.  
- `{1,5}` - **1 to 5** times.  
-  `{2,}` - **2 or more** times.  
- `*` - **0 or more** times.  
- `+` - **1 or more** times.

For example, look at these regex : 

- `cats{4}` : Match the string catssss
- `[Cc]ats*` : Match the words Cat, cats, catsss
- `regex go br+` : Match the sentences regex go br, regex go brrrrrr and so on



## Starts with/ ends with, groups, and either/ or

Sometimes it's very useful to specify that we want to search by a certain pattern **in the beginning or the end of a line**. We do that with these characters:  

- `^` - starts with  
- `$` - ends with

So for example, if you want to search for a line that **starts with** `abc`, you can use `^abc`.  
If you want to search for a line that **ends with** `xyz`, you can use `xyz$`.

Note: The `^` hat symbol is used to exclude a charset when enclosed in `[`square brackets`]`, but when it is not, it is used to specify the beginning of a word.

You can also define groups by enclosing a pattern in `(`parentheses`)`. This function can be used for many ways that are not in the scope of this tutorial. We will use it to define an **either/ or** pattern, and also to repeat patterns. To say "or" in Regex, we use the `|` pipe.

For an "either/or" pattern example, the pattern `during the (day|night)` will match both of these sentences: `during the day` and `during the night`.  
For a repetition example, the pattern `(no){5}` will match the sentence `nonononono`.

## Ready-to-use regex
You can find here a list of regex you can use to search valuable informations within any files
### How to use them ? 
#### GUI text editor
When searching for text, select the mode "Regular expression" and copy paste the regex of your choice in the field "Search"
#### CLI
##### Linux
###### grep command
```bash
grep "your_regex" filename 
grep -a "your_regex" binaryfilename
```
###### sed command
```bash
sed -n '/your_regex/p' filename 
```
#### Programming language
### Cryptographic data
These regex allow you to find data regarding any cryptographic data such as public, private key and certificate in differents format
#### Public key
This section regroups all regex which allow you to find public key anywhere
??? faq "Public key"

	=== "Classic public key"
		```
		(-{3,}BEGIN PUBLIC KEY)-{3,}\n([\s\S]*?)\n-{3,}(END PUBLIC KEY-{3,})
		```
	=== "RSA public key"
		```
		(-{3,}BEGIN RSA PUBLIC KEY)-{3,}\n([\s\S]*?)\n-{3,}(END RSA PUBLIC KEY-{3,})
		```

#### Private key
??? faq "Private key"

	=== "Openssh private key"
		```
		(-{3,}BEGIN OPENSSH PRIVATE KEY)-{3,}\n([\s\S]*?)\n-{3,}(END OPENSSH PRIVATE KEY-{3,})
		```
	=== "Classic private key"
		```
		(-{3,}BEGIN PRIVATE KEY)-{3,}\n([\s\S]*?)\n-{3,}(END PRIVATE KEY-{3,})
		```
	=== "RSA private key"
		```
		(-{3,}BEGIN RSA PRIVATE KEY)-{3,}\n([\s\S]*?)\n-{3,}(END RSA PRIVATE KEY-{3,})
		```


#### Certificate

??? faq "Certificate"

	=== "Certificate with line break"
		```
		(-{1,}BEGIN CERTIFICATE)-{1,}\n([\s\S]*?)\n-{1,}(END CERTIFICATE-{1,})
		```
	=== "Certificate without line break"
		```
		(-{1,}BEGIN CERTIFICATE)-{1,}\n?([\s\S]*?)\n?-{1,}(END CERTIFICATE-{1,})
		```


#### Hash method
??? faq "Hash method"

	=== "MD5"
		```
		^[0-9a-fA-F]{32}$
		```
	=== "SHA1"
		```
		/\b([a-f0-9]{40})\b/
		```
	=== "SHA256"
		```
		^[0-9a-fA-F]{64}$
		```

### Cryptocurrency
#### Bitcoin
??? faq "Cryptocurrency wallet"

	=== "Bitcoin Wallet"
		```
		/^(bc1|[13])[a-zA-HJ-NP-Z0-9]{25,39}$
		```

### Encoding methods

??? faq "Encoding methods"

	=== "Base64"
		```
		(?:[A-Za-z0-9+\/]{4})*(?:[A-Za-z0-9+\/]{4}|[A-Za-z0-9+\/]{3}=|[A-Za-z0-9+\/]{2}={2})
		```

### Network information

??? faq "L2 info"

	=== "MAC address"
		```
		/^((([a-f0-9]{2}:){5})|(([a-f0-9]{2}-){5}))[a-f0-9]{2}$/i
		```

??? faq "L3 info"

	=== "IPv4 address"
		```
		(\d{1,3}\.){3}\d{1,3}
		```
	=== "IPv6 address"
		```
		^(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))$
		```

??? faq "L7 info"

	=== "URL"
		```
		((https?|ftp|file):\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?
		```

	=== "Domain name"
		```
		([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?
		```


### General informations
#### Email address
??? faq "Email address"
	```
	(\w+)@(\w+)\.([a-zA-Z]{1,10})
	```

#### Phone number

??? faq "Phone number regex"

    === ":flag_fr:"
		```
		/^(?:(?:\+|00)33[\s.-]{0,3}(?:\(0\)[\s.-]{0,3})?|0)[1-9](?:(?:[\s.-]?\d{2}){4}|\d{2}(?:[\s.-]?\d{3}){2})$/
		```
    === ":flag_gb:"
		```
		/^(((\+44\s?\d{4}|\(?0\d{4}\)?)\s?\d{3}\s?\d{3})|((\+44\s?\d{3}|\(?0\d{3}\)?)\s?\d{3}\s?\d{4})|((\+44\s?\d{2}|\(?0\d{2}\)?)\s?\d{4}\s?\d{4}))(\s?\#(\d{4}|\d{3}))?$/
		```
    === ":flag_ge:"
		```
		/(\(?([\d \-\)\–\+\/\(]+){6,}\)?([ .\-–\/]?)([\d]+))/
		```
    === ":flag_us:"
		```
		/^\\(?([0-9]{3})\\)?[-.\\s]?([0-9]{3})[-.\\s]?([0-9]{4})$/
		```
    === ":flag_cn:"
		```
		/^(?:(?:\d{3}-)?\d{8}|^(?:\d{4}-)?\d{7,8})(?:-\d+)?$/
		```
    === ":flag_in:"
		```
		/^(?:(?:\+|00)86)?1(?:(?:3[\d])|(?:4[5-79])|(?:5[0-35-9])|(?:6[5-7])|(?:7[0-8])|(?:8[\d])|(?:9[189]))\d{8}$/
		```
		```
		/((\+*)((0[ -]*)*|((91 )*))((\d{12})+|(\d{10})+))|\d{5}([- ]*)\d{6}/
		```
    === ":flag_br:"
		```
		/\(([0-9]{2}|0{1}((x|[0-9]){2}[0-9]{2}))\)\s*[0-9]{3,4}[- ]*[0-9]{4}/
		```
    === ":flag_au:"
		```
		/(^1300\d{6}$)|(^1800|1900|1902\d{6}$)|(^0[2|3|7|8]{1}[0-9]{8}$)|(^13\d{4}$)|(^04\d{2,3}\d{6}$)/
		```
    === ":flag_nl:"
		```
		/(^\+[0-9]{2}|^\+[0-9]{2}\(0\)|^\(\+[0-9]{2}\)\(0\)|^00[0-9]{2}|^0)([0-9]{9}$|[0-9\-\s]{10}$)/
		```
    === ":flag_se:"
		```
		/^(([+]\d{2}[ ][1-9]\d{0,2}[ ])|([0]\d{1,3}[-]))((\d{2}([ ]\d{2}){2})|(\d{3}([ ]\d{3})*([ ]\d{2})+))$/
		```

## Resources
### Practice regex

If you want to learn more about regex, please check this tryhackme room : [This is a very nice room](https://tryhackme.com/room/catregex), All the information regarding the regular expressions come from this very same room. 
You can also test your regex [here](https://regexr.com/) and [here also](https://regex101.com/)

### Regex collections
https://github.com/dubniczky/Regex-Collection
https://github.com/aloisdg/awesome-regex
https://regexpattern.com

