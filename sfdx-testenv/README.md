# sfdx-cli

Base client for sfdx testing.  Includes sfdx client and chrome.

## example

Example CI Script

```
before_script:

  - mkdir assets
  - mkdir -p ~/.sfdx/dev  # this works around a bug in sfdx where you cannot have / in the username, and we have some set up as dev/
  - echo -e $SERVER_KEY | base64 -d > assets/server.key
  - export SFDX_AUTOUPDATE_DISABLE=true
  - export SFDX_USE_GENERIC_UNIX_KEYCHAIN=true
  - export SFDX_DOMAIN_RETRY=300
  - sfdx --version
  - sfdx plugins --core
  - sfdx force:auth:jwt:grant --clientid "$CONSUMERKEY"  --username "$USERNAME" --jwtkeyfile "assets/server.key" --setdefaultdevhubusername -a DevHub
```

```
script:

 - sfdx force:org:create -v DevHub -s -f config/project-scratch-def.json -a ciorg
 - sfdx force:org:list
 - lintout=$(sfdx force:lightning:lint force-app/main/default/aura/ --config config/eslintrc.json);if [[ $lintout =~ .*problem.*error.*warning ]]; then echo "$lintout"; false; fi
 - sfdx force:source:push -u ciorg
 - sfdx force:apex:test:run -u ciorg -c -r human --wait 10
 - sfdx force:lightning:test:run -f config/lts.json -u ciorg  -a icApplicationTest  -d out -t 1000000
 
```
