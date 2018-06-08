## Install and Setup Development Environment

### Prerequisites

1. Install node v9.4.0 or greater. (We recommend [NVM](https://github.com/creationix/nvm))
1. Install the [SFDX CLI](https://developer.salesforce.com/tools/sfdxcli)
1. Install typescript using `npm install -g typescript`
1. Install gulp-cli using `npm install --global gulp-cli`
1. If you do not have write access and need to make changes, ask platform-cli group on slack or the alm-ci team on chatter.

### Develpoment Steps

1. Clone this repository. `git clone git@git.soma.salesforce.com:ALMSourceDrivenDev/force-com-toolbelt.git`
1. `cd force-com-toolbelt`
1. Switch to the correct branch: `git checkout release` 
1. Run `npm install` to bring in all the dependencies.
1. Run `gulp` to transpile your changes and output them in the dist folder.
1. Run `sfdx plugins:link .` to link the plugin in the CLI
    1. `sfdx plugins` to verify you have a *(link)*ed plugin.  You should see something like `salesforce-alm 43.0.26-0 (link)`

    
