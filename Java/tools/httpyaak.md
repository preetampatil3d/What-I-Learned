### HTTPYAAK

VSCODE : 
Create .http request file which calls rest api with body

requestBody.http 
 ```
### NameOfTheRequest
# [KB] Comments
# @loop for 1
POST path/to/api 
< /path/to/input/json/for/api 
#OR we can include json here as well
{ }
```

###  Inject variable in request body.
example of .http request where we can inject variable. when variable is passed from CLI or created variable in .http file, user need to add # @injectVariables
```
# @injectVariables
@foo=bar -- This is not required when passing variable from CLI
POST
```
example of request body json file 
{ "name" : "{{foo}}"}


### CLI UTIL:
Eaither access httpyac from global directory "/projects/dir/.npm-global/./httpyac"
or we can access it from npm package npm run httpyac 
```
# Install
npm install -g httpyac

# usage
httpyac [.http file path] [options]
[options]
--repeat n # repeat request for n number
--all # execute all request from .http file 
--name NameOfTheRequest # Execute specific request 
--json # response in json format 
--output short #
```

### Eclipse:
Run cli command/ external util from eclipse
1. Open External Tool Configuration > New
2. 
	- Location: path to installation of util. for node, we should look for node.exe file location which is present in ProgramFile/Node/node.exe 
	- Working Directory: where we want execute this command 
	- Argument: For Node commands , node-cli.js is used to execute commands. which is present in node_module\npm\bin\npm-cli.js
		- node-cli.js  ${string_prompt:Description (It will give popup where we can add value):"defaultValue"}

3. file is created at <workspace>/.metadata/.plugins/org.eclipse.debug.core/.launches
	- Copy it your workspace, So that i will available 
