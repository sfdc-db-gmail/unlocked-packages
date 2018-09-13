# FAQ on Unlocked Packages (Salesforce Customers and non-ISV partners)

(Click [here](../intro.md) to go back to the main page)

<h2 id="what-is-packaging">
What are Unlocked Packages?
</h2>

As part of Salesforce DX, with the Winter '19 release, we offer **Unlocked Packages** as a Generally Available (GA) product. We’re bringing you, our enterprise customers and non-ISV partners (SIs and consultants), a new type of packaging solution, designed with you and your use cases in mind.

Unlocked Packages offer a new mechanism for deploying  metadata to Salesforce orgs. We believe they provide distinct advantages over current technologies like Change Sets and the ANT Migration Tool (See [here](#benefits-of-packaging) for the benefits of unlocked packages over current technologies).

Among other things, unlocked packages attempt to answer  two key questions:

1. How does package development make it easier for me to package, deploy, and maintain new apps on the Salesforce Platform?
2. How can I organize my existing unpackaged metadata in production orgs to adopt modular development practices that Salesforce DX prescribes?

Before we move ahead, it helps to clarify what we mean by unpackaged metadata.

Broadly speaking, the metadata in your production org can be categorized into three buckets:

1. The set of metadata that Salesforce provides - such as  Sales Cloud, Service Cloud, etc. The Account object or the Case object falls under this category.
2. Metadata that is part of AppExchange packages installed in the production org.
3. Other metadata. This can be a Custom Field or Apex Trigger that you added to the Account object, the customizations that you made to an AppExchange app, or a brand new app that your team built in-house. This is the sum-total of all custom stuff that you have built over the years in your org. This category of metadata is referred to as *unpackaged metadata* in this FAQ.

It also helps to clarify what we mean by package and package version.

A Package is a container for Salesforce metadata. You can add metadata to a package and take a snapshot of it any time. This snapshot is called a package version. You can create package versions any number of times. Each package version, once created, is immutable. You can install a package version in any Salesforce environment - scratch orgs, sandbox orgs, or production orgs. Installing a package version deploys the metadata that was specified when the package version was created.

<h2 id="benefits-of-packaging">
What are the benefits of packaging over Change Sets and ANT Migration Tool?
</h2>

The following are some key benefits of packaging:

1. The metadata that you specify when you create a package version is the same metadata that is contained in your Salesforce DX project and version control system. Packages promote the source-driven development approach and use the source format that Salesforce DX uses.
2. With package versions, you have an immutable, versionable artifact that can be used in CI, UAT, etc. The same artifact that passes all your CI tests and UAT can be installed in your production orgs.
3. Packages promote modular development where you can organize your functionality into logical, modular, interdependent units, and use packaging for deployment and organization. You get all the good things that come with modular, iterative development practices.
4. You can express dependencies among packages. With this capability, you can embrace a modular approach where each package represents a set of metadata that represents a distinct unit of functionality with well-defined dependencies among packages.
5. Packages can be tracked in the installed org because they show up as installed packages with a set of associated metadata. In the production org, for a given metadata, you can view the package it is associated with, and for a given package, you can view the set of all metadata owned by the package. Whether it’s during staging or deployment, Packages ease the task of tracking what is part of an artifact compared to existing technologies like change sets.
6. Packages provide a repeatable, scriptable and trackable way to manage change as you develop functionality using Salesforce DX.

<h2 id="package-types">
What are the different types of packages and which one should I use?
</h2>

See [here](./faq-packages.md/#types-of-packages) for the answer.

<h2 id="hello-world">
How do I build and deploy a new app using unlocked packages? What’s my Hello World App for unlocked packages?
</h2>

In less than 5-10 minutes, you can build and test your Hello World App to get a feel for unlocked packages. The only precondition is that you have a Salesforce DX environment set up with Salesforce CLI and that you have enabled Dev Hub and Unlocked Packages in your Dev Hub Org. 

If you haven’t done it already, refer to this [documentation](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm) for setting up De Hub and Packaging.

Follow these simple steps and you have for yourself a package!


1. Set up a directory on your local machine and copy the sample repo. Let’s presume this is the app that your team is building for meeting a business need.
   
    ```git clone https://github.com/dreamhouseapp/dreamhouse-sfdx.git```  

    ```cd dreamhouse-sfdx/```  

    (If you want more info about the DreamHouse app, see here)


2. Authenticate to your Dev Hub org.  
```sfdx force:auth:web:login --setdefaultdevhubusername```


3. Create a new unlocked package.  
```sfdx force:package:create --name Dreamhouse-App --packagetype Unlocked --path force-app --nonamespace```  
Note how the sfdx-project.json is automatically updated with information about this package.


4. Create a package version. When you create a package version, you are associating metadata with your package. Over the course of time, you create many, many package versions for a given package. Once created, a package version serves as an immutable artifact containing a specific set of metadata. This is the same metadata that you specify at the package version creation step.   
```sfdx force:package:version:create --package Dreamhouse-app --installationkey mypkgisnowsecure#%^ --wait 20```

    In this example, force-app is the directory in which your package metadata is located in Salesforce DX format. The other pieces of information needed for package version creation are stored in sfdx-project.json.


5. Create a scratch org where you can install the package.  
```sfdx force:org:create --definitionfile config/project-scratch-def.json --setalias sub1 --setdefaultusername```


6. Install the package in the scratch org.  
```sfdx force:package:install --package Dreamhouse-app@0.1.0-1 --publishwait 20 --wait 10```  

If `Dreamhouse-app@0.1.0-1` is not the alias for the package version created in step 4, update the value for the `--package` flag with the correct alias.


7. Open the scratch org in the browser.  
```sfdx force:org:open```


8. In the scratch org, go to Setup -> Installed Packages, where you can see that the package has been installed.


9. Click the Package Name and the View Components button to validate that the components present in the project are now deployed as part of the package.


Congratulations! You have now created and installed your first unlocked package!


<h3 id="organize-md">
Q. How can I organize my unpackaged metadata using DCP?
</h3>
**Step 1** - Extract a small set of unpackaged, self-contained metadata from your production org and convert it to Salesforce DX format using mdapi:retrieve and mdapi:convert commands.

How to identify this set of self-contained metadata is a bit of a science and a bit of an art. You can try and select metadata that represents an app, or customization to an AppExchange app, or customizations to Sales Cloud or an Account object, or some set of metadata that represents a functional unit (e.g.: a library of apex classes). We know this is a challenging task. We plan to produce material to help in this. See this [blog](https://developer.salesforce.com/blogs/2018/02/getting-started-salesforce-dx-part-1-5.html) series that talks about DX and unlocked packages.

**Step 2** - Push this metadata to a scratch org using source:push and validate that this is the metadata that you want to be part of a new package. If you are missing some metadata, go back to Step 1.

**Step 3** - Create an unlocked package using this metadata (see [here](#hello-world) for how to create a package). Make sure this package has no namespace (supply the --nonamespace flag to the force:package:create command). See here for more info about namespaces and packages.

**Step 4** - Test the package in CI and UAT environments. If you encounter issues, you may have to go back to Step 1 and validate that you have the right set of metadata. Once the package has passed all CI runs and UAT on sandboxes, install the package in the production org. This installation does NOT deploy new metadata as what’s contained in the package is already present in the production org. But what this does is “migrate” the metadata from the unpackaged set to the package so that it is now part of the package in the production org. From this point forward, you can iterate over this metadata set using the package.

These steps, along with the specification of dependencies among packages, can help in organizing all of the unpackaged metadata into a set of interdependent packages.

<h2 id="common-pkg-ops">
Q. What are some of the common packaging operations?
</h2>

**Package Creation**  
A package is a container of metadata. When you create a package, you specify some characteristic info about the package: name, description, metadata and namespace associated with it, package type and so on. 

**Package Version Creation**  
As your development team is working on some functionality, you, as a release manager, may want to take periodic snapshots of the functionality when it is in a functional state. Such snapshots are package versions. When the package version creation operation is executed, a copy of the current state of the metadata in the project folder (the directory specified in `force:package:create` command) is used to create the package version. You can create any number of package versions. Each version, once created, is immutable.

**Package Install**  
A package install is an operation whereby the metadata of a package is deployed to the target org. A package install is actually a package version install - it is the deployment of metadata contained in a specific package version of a package. A package is typically associated with 100s or even 1,000s of package versions. When you install a package, you specify which one of the many versions you intend to install. Install and deploy mean the same in this context.

**Package Upgrade**  
Installing a version on top of a previously installed package version is a package upgrade. When you perform a package upgrade, the metadata of the new package version is deployed to the org. The package upgrade process can add new metadata to the org, modify existing metadata, and may even delete and deprecate some metadata depending on what’s there in the new version of the package.

**Package Downgrade**  
See here for more info about package downgrades.

**Package Uninstall**  
This is an operation where you are deleting the metadata and associated data related to an installed package.

See here for how you can perform these operations using the Salesforce CLI.

<h2 id="simple-json">
Q. How can I specify attributes in the sfdx-project.json file for managing my packages?
</h2>

Some of the sfdx-project.json updates are done automatically for you. See here for more info. 

See this example below for a section of sfdx-project.json file

(this deliberately avoids package dependencies to keep things simple. See here for a simple example of package dependencies and here for a more complex scenario)  

``` "packageDirectories": [

       {

           "package": "revenue-app",

           "path": "common",

           "default": true,

           "versionNumber": "0.1.0.NEXT",

           "versionName": "Summer 2018",

           "versionDescription": "Summer 2018 Release",

       },

       {

           "package": "revenue-app-utils",

          …

       }

],

    "namespace": "",
    "sfdcLoginUrl": "https://login.salesforce.com",
    "sourceApiVersion": "43.0",


packageAliases {
        "revenue-app": "0HoB00000008OoZKAU",
        "revenue-app-utils": "0HoB00000008OoYMZD"
    }
```
package (required field) - this is the package id that starts with 0Ho. The output of the force:package2:create command provides this ID.

path (required field) - root folder where the source metadata of the package is stored in the local workspace in Salesforce DX format

versionName (required field) and versionDescription (optional field) - name and description of the package version that you’re creating.

versionNumber  (required field) - version number in major.minor.patch.build format where each one of these is a whole number. See here for more info about package versions.


<h2 id="supported-md">
Q. What types of metadata are supported in an unlocked package?
</h2>

Most metadata that is supported by Salesforce DX is supported in unlocked packages. See [here](https://mdcoverage.secure.force.com) for more information.

<h2 id="namespace">
What is a namespace and should I use one with my unlocked package?
</h2>

A namespace is a unique identifier that is owned by your company when you register it with Salesforce. If you associate a namespace (like Acme) with a package (see here for how to associate namespaces with Dev Hub), the API names of all metadata added to the package will have the prefix Acme__. Hence, namespaces offer a mechanism to name and organize metadata in your org.


Keep the following in mind when you think of namespaces and packages:

1. Namespaces are optional for unlocked packages. There are many use cases where you won’t want to use namespaces with unlocked packages. If you intend to move your metadata from unpackaged state in your production org to unlocked packages (see here for more info), create packages without a namespace.
2. A Salesforce DX Dev Hub can be linked to multiple namespaces so it’s possible for you to use different namespaces for different packages.
3. Multiple packages can be associated with the same namespace.
4. A package can be associated with only one namespace.
5. A package can be associated with a namespace only during package creation (`force:package:create`) and cannot be changed, once associated.
6. One of the known issues with namespaced unlocked packages is that the Apex in such a package is hidden and the experience associated with debug logs is sub-optimal. It is on the roadmap to fix this but the earliest that can happen in Summer '19.

<h2 id="namespace">
How do I iterate on my package? What happens when I add, edit and delete package metadata?
</h2>

Unlocked Packages provide a very open framework for making metadata changes. Before we talk about these changes, make sure you understand the difference between a package and a package version. See here for more info. Also take a look at [this](#common-pkg-ops) for packaging operations.


Versioning is one of the core differentiating features of packaging. It is versioning that enables iterative development, where you can continually update an artifact (package) in a controlled and manageable way in response to business needs (feature requests and bug fixes).


When it comes to versioning and changes introduced in versions, it is helpful to look at them from two angles - development stage and deployment stage.


**Development and Build Stages**

When you create a new package version, you can introduce the following changes in relation to a previous package version:

1. Add new metadata to the project workspace, such as  a new Lightning Page or a new Apex Controller
2. Modify existing package metadata, such as updating a business process in response to end-user feedback or updating an Apex method to fix a bug.
3. Delete existing package metadata, such as deleting Visualforce pages as they are being replaced by Lightning Pages, deleting a custom field as it has become obsolete, etc.

The above-described changes can be applied in a variety of ways (Setup UI in scratch orgs, in VS Code, in Developer Console, etc.) and ultimately make it to the version control system for the project (See here for relationship between version control system and packages). The release manager then pulls in the right set of metadata to the local workspace and creates a new package version so that the package version captures these changes.


**Deployment Stage**

Let’s presume you are trying to deploy (aka install) v 2.0 of a DCP in an environment where v 1.0 is currently installed. During this package upgrade process, the following changes are applied:

1. Metadata that is new in v2.0 in comparison to v1.0 are deployed or created in the target org.
2. Metadata that has changed are applied. For example, if a Lightning Page or Apex Controller has been modified in v2.0, those modifications are applied as part of package upgrade.
3. When it comes to metadata delete - this is the scenario where something (custom field or report or apex class) present in v1.0 was deleted in v2.0 - there are two possible outcomes:
   * For the first class of metadata types, the metadata is deleted from the target org.
   * For the second class of metadata types, the metadata is marked as deprecated. When metadata is marked deprecated, it is still part of the package but it shows up as deprecated when you look at the metadata in Setup so that it is easier for admins to track and delete them from the org. We understand that this deletion of metadata in the installed org is a manual step that should be automated. In many scenarios, you may want the package upgrade process to automatically delete metadata that was removed from the package. We plan to make this process more automatable in future releases.





