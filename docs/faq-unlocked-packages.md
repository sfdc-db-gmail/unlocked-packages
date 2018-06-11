# FAQ on Unlocked Packages (Salesforce Customers and non-ISV partners)

(Click [here](../intro.md) to go back to the main page)

<h3 id="what-is-packaging">
What is Packaging?
</h3>
As part of Salesforce DX, we offer **Unlocked Packages** as an open beta product. We’re bringing you, our enterprise customers and non-ISV partners (SIs and consultants), a new type of packaging option, designed with you and your use cases in mind.

Unlocked Packages offer a new mechanism to deploy your metadata to Salesforce orgs. We believe they provide distinct advantages over current technologies like change sets and the ANT Migration Tool (See [here](#benefits-of-packaging) for the benefits of DCPs over current technologies).

Among other things, unlocked packages try to answer these two key questions:

1. How does package development make it easier for me to package, deploy, and maintain new apps on the Salesforce Platform?
2. How can I organize my existing unpackaged metadata in production orgs to adopt modular development practices that Salesforce DX prescribes?

Before we move ahead, it helps to clarify what we mean by unpackaged metadata.

Broadly speaking, the metadata in your production org can be categorized into three buckets:

The set of metadata that Salesforce provides - such as  Sales Cloud, Service Cloud, etc. The Account object or the Case object falls under this category.
Metadata that is part of AppExchange packages installed in the production org.
Other metadata. This can be a Custom Field or Apex Trigger that you added to the Account object, the customizations that you made to an AppExchange app, or a brand new app that your team built in-house. This is the sum-total of all custom stuff that you have built over the years in your org. This category of metadata is referred to as unpackaged metadata in this FAQ.

It also helps to clarify what we mean by package and package version.

A Package is a container for Salesforce metadata. You can add metadata to a package and take a snapshot of it any time. This snapshot is called a package version. You can create package versions any number of times. Each package version, once created, is immutable. You can install a package version in any Salesforce environment - scratch orgs, sandbox orgs, or production orgs. Installing a package version deploys the metadata that was specified when the package version was created.

<h3 id="benefits-of-packaging">
What are the benefits of packaging over change sets and ANT Migration Tool?
</h3>

The following are some key benefits of packaging:

1. The metadata that you specify when you create a package version is the same metadata that is contained in your Salesforce DX project and version control system. Packages promote the source-driven development approach and use the metadata format that Salesforce DX uses.
2. With package versions, you have an immutable, versionable artifact that can be used in CI, UAT, etc. The same artifact that passes all your CI tests and UAT can be installed in your production orgs.
3. Packages promote modular development where you can organize your functionality into logical, modular, interdependent units, and use packaging for deployment and organization. You get all the good things that come with modular, iterative development practices.
4. You can express dependencies among packages. With this capability, you can embrace a modular approach where each package represents a set of metadata that represents a distinct unit of functionality.
5. Packages can be tracked in the installed org because they show up as installed packages with a set of associated metadata. So, in the production org, you can view which metadata belongs to which package. Whether it’s during staging or deployment, Packages ease the task of tracking what is part of an artifact compared to existing technologies like change sets.
6. Packages provide a repeatable, scriptable and trackable way to manage change as you develop functionality using Salesforce DX.

