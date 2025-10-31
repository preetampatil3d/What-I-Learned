# Mac JDK Installation with jenv

## Install brew and jenv
``` 
brew upgrade
brew install jenv
```
## Install jdk version and add to jenv
```
brew install openjdk
jenv add /opt/homebrew/opt/openjdk/libexec/openjdk.jdk/Contents/Home

brew install openjdk@21
jenv add /opt/homebrew/opt/openjdk@21
```

## validate
```
jevn version
jevn versions
jenv doctor
```

## Enable once installation is complete
```
jenv enable-plugin export
jenv enable-plugin maven
jenv enable-plugin gradle
jenv enable-plugin exec
```

## Set global/local jdk version
```
jenv versions

jenv global <version>
jenv local <version>
jenv shell <version> # temporay
```


### restart the shell or run the below command to reflect jenv changes in current shell
```
exec "$SHELL"
```