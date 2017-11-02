#Sitecore Habitat

Habitat is a Sitecore solution example built on a modular architecture.
The architecture and methodology focuses on:

* Simplicity - *A consistent and discoverable architecture*
* Flexibility - *Change and add quickly and without worry*
* Extensibility - *Simply add new features without steep learning curve*

For more information, please check out the [Habitat Wiki](https://github.com/Sitecore/Habitat/wiki)

## How to Increase Build and Deployment Times ##
When creating your own Helix-based solutions, please consider some of the ways that you can improve build and deployment times. Some possible solutions for this can be found here:- http://sitecore.stackexchange.com/questions/4076/slow-build-performance-using-tds-helix-inspired/4114

## Differences from the [original repo](https://github.com/Sitecore/Habitat) ##

### Serialization and Deployment ###
This version of Habitat is using Team Development For Sitecore (TDS) for Item Serialization and Deployments.

TDS is an Enterprise-grade, Sitecore development and deployment tool created by Hedgehog Development. At its core, it provides companies with the ability to automate their Sitecore builds or set up a continuous deployment scenarios. TDS provides several additional features its users find valuable, for more information visit: www.teamdevelopmentforsitecore.com.

### Config Transforms ###
Config transforms are implemented through XDT transforms using Visual Studio builds and TDS's in-build transforming capabilities. This works similar to the popular Visual Studio extension, Slow Cheetah.
 - The Sitecore, out-of-the-box App_Config/Security/Domains.config and Web.config are included in the solution (in the Project.Habitat module)
 - App_Config/Security/Domains.config is patched during a build using the Domains.[Configuration].config file. This transform file includes the transforms from Foundation.Accounts and Project.Habitat from the original repo.
 - Web.config is patched, and directly has the transforms from the Foundation.Forms, Foundation.Installer and Project.Common modules in it.

### Build Order ###
In order for the solution to deploy correctly, we needed to tell it to deploy in the order that the [Sitecore Helix](http://helix.sitecore.net/) principles dictate. This order is generally Foundation modules, then Feature modules, then Project modules.
As some Foundation modules rely on other Foundation modules, it is required that those dependent modules are deployed first.
It is noted that the Serialization, SitecoreExtensions, Assets, Dictionary and Indexing modules need to be deployed first, as other Foundation modules rely on them.
After that we have enforced the deployment of the Foundation modules alphaetically, followed by the Feature modules alphabetically, followed by Project.Common, and then Project.Habitat.
The Test projects are then deployed, in order of Foundation (alphabetical) then Feature (alphabetical).

This Build Order is setup by explicitly adding dependencies to previous modules using Visual Studio's 'Build Dependencies -> Project Dependencies' dialog.
In order to achieve the appropriate order, each module's TDS project is explicitly set to depend on the previous module's TDS project. i.e Feature.Media.Master.scproj will depend on Feature.Maps.Master.scproj. Therefore, explicitly added dependencies are only linked between TDS projects, not code projects.

## Installation: ##

1. Clone this repository to your local file system.
2. Set up a clean Sitecore 8.2 Update-1 website (We recommend using Sitecore Instance Manager). The codebase uses this version with it's referenced NuGet packages and configs, so using any other Sitecore version may require changes to the code.
 - Default URL: http://habitat.dev.local/ 
 - Default Location: C:\Websites\Habitat.local\
3. Install the Webforms for Marketers module.
4. Open the solution in Visual Studio.
5. (optional) Configure your settings if you are using other settings than default:
To change the standard location of source, website files and website URL modify the following files:
  - /Configuration/z.Habitat.DevSettings.config
  - /Configuration/TdsGlobal.config 
6. Deploy the Solution (Right click on the solution -> Deploy Solution) in Visual Studio 2015. This will restore all NuGet packages, build the code, and deploy all Sitecore Items and Project Items to your local website.
7. Be productive!
