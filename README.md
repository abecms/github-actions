# Livingcolor Actions

## retrieve-locale-files-fr.yml

### Use Case

1. On Shopify or ... users always contribute the translations in production, resulting in json files modified...
we'd like to have the translation always up to date on every environment (development, staging, ...).
We see these modifications as code, so we want to sync it from prod to dev.

### Description
this action listens to a branch for specific files changed
If it's the case then commit it on another branch


## Shopify theme 2.0 recipe

### Use Case

With Shopify you can now attach your theme to a github branch. Unfortunately they don't use .gitignore. Leading to inconsistencies between environment. Example: 
We have Shopify dev store and shopify production store. The `settings_data.json` is proper to each env.
But the dev `settings_data.json` is always pushed by shopify in the dev branch no matter what is included in the .gitignore.
when we deploy dev to production, we overwrite the prod `settings_data.json` by the dev `settings_data.json` !!!

The solution ??? none !! (https://community.shopify.com/c/technical-q-a/github-integration-doesn-t-support-the-gitignore-file/m-p/1253056)

So here we are :)

Be prepared !
                                                                main-hk
                                                                main-jp
                                                                main-uk
1. branch dev => deploy-staging => staging => deploy-prod =>    main-fr
                                                                main-de
                                                                main-eu
                                                                main-it
                                                                main-es
                                                                main-int

2. action shopify-deploy-staging - 
    * avoid to overwrite theses files : 
        config/settings_data.json
        assets/zifyapp-fblogin.js
        templates/*.json
        assets/algolia_config.js
        assets/algolia_config.js.liquid
        ...
    * then deploy to staging branch

3. action shopify-deploy-prod 
    * avoid to overwrite these files : 
        config/settings_data.json
        assets/zifyapp-fblogin.js
        templates/*.json
        assets/algolia_config.js
        assets/algolia_config.js.liquid
        ...
    * avoid to overwrite the locale files except :
        default
    we execpt it because we want to add keys translation

4. never use the default to translate but create a specific json translation file

5. deploy to all environment (stores)
