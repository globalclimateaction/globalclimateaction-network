# About / À propos

## About

This project was financed by the French Ministry of Ecology as part of the COP21 conference on climate, in order to share information on the climate initiatives. It was developped for the ministry by XWiki SAS in 2016 and published as an Open Source project in 2021, under the LGPL 2.1 licence.

## À propos

Ce projet a été financé par le Ministère de l'Écologie, de l'Énergie, du Développement durable et de la Mer dans le cadre de la COP21 afin de partager de l'information sur les initiatives sur le climat. Il a été développé pour le ministère par la société XWiki SAS en 2016 et mis en Open Source en 2021, sous licence LGPL 2.1.

# Installation instructions

## XWiki Platform base

This project is an XWiki application, which means that it needs to be installed on top of a running instance of the XWiki Platform. See https://www.xwiki.org/xwiki/bin/view/Download/ about how to get and configure such an instance. Note: during this installation you may be asked to create an administrator account. For ideal results with the current application, you should choose ```Admin``` as the account name for this administrator account (case sensitive).

The code of this branch was briefly validated for XWiki Standard version 12.10.8, the current LTS version of the XWiki project. The original development of this application was done using XWiki Standard version 8.2.1 as base. The code compatible with the 8.2.1 version of XWiki can be found on the dedicated 8.2.1 branch.

## Building the sources

In order to install this application, the sources above need to be built into deliverable packages with Maven. See the documentation on xwiki.org about how to configure Maven for building XWiki modules: https://dev.xwiki.org/xwiki/bin/view/Community/Building/#HInstallingMaven . The current build was tested with version 3.5.2 of Maven.
The following artefacts will result from the build and will be used for the installation:
* ```xwiki-gcaa-wikis-main-<version>.xar``` , built from the module ```wikis/main``` . This module contains the code of the application.
* ```xwiki-gcaa-wikis-data-main-<version>.xar``` , built from the module ```wikis/main-data``` . This module contains some demo content for the application.

## Installation prerequisites

Besides the modules described above, for the proper functioning of the application, a couple of extensions need to be installed on the XWiki platform before the installation of the modules above. This can be done from the Administration of the wiki, the "Extensions" section (see documentation of this section here https://extensions.xwiki.org/xwiki/bin/view/Extension/Extension%20Manager%20Application ).
* The Blog application, version ```8.2.1```. The simplest way to install this version is by using the "Advanced search" feature of the Extension manager and typing in the extension id and version: ```org.xwiki.contrib.blog:application-blog-ui``` , version ```8.2.1```.
  * The blog category ```Blog.Personal``` should be deleted from the blog after installation of the application.
* The Mocca Calendar application, version 2.5.3. The simplest way to install this version is by using the "Advanced search" feature of the Extension manager and typing in the extension id and version: ```org.xwiki.contrib:application-mocca-calendar-ui``` , version ```2.5.3``` .
  * The pages ```MoccaCalendar.Events``` and ```MoccaCalendar.MoccaCalendarTemplate``` should be deleted after the installation of this extension .

## Installation

### Code

In the XWiki Administration, in the "Content" -> "Import" section (described here https://extensions.xwiki.org/xwiki/bin/view/Extension/Administration%20Application#HImport ), upload and import the pages in the package ```xwiki-gcaa-wikis-main-<version>.xar``` , built as described above.

Since this package contains the factory settings of the XWiki platform configuration, if this is not the first installation of the project, you may want to import the ```xwiki-gcaa-wikis-main-upgrade-<version>.xar``` package (built from the ```wikis/main-upgrade``` module), which contains the same code as the original package except for these factory settings of the configurations.

### Demo data

The main-data package contains a couple of pages of demo/initialization content (13 sectors and a page template to create a new sector, a couple of Categories for the blog application, all the data describing the geographical regions, etc.) Although the application is ready to use after the installation of the code, you probably want to import this demo/initialization package as well, in order to get going faster, knowing that you can always delete any of the contents brought in by this package, if needed.

In the XWiki Administration, in the "Content" -> "Import" section (described here https://extensions.xwiki.org/xwiki/bin/view/Extension/Administration%20Application#HImport ), upload and import the pages in the package ```xwiki-gcaa-wikis-data-main-<version>.xar``` , built as described above.

# Usage

Once these steps are done, go to the homepage of the wiki (by clicking the logo) and let yourself guided - following the menu entries - for creating new content and editing existing content.
By the factory settings of this application, regular users can only view the pages; in order to create new content, change rights or add new users in the contributor groups, login with an administrator account.

## Administration operation - creating a new sector

The creation of a new sector is not a straightforward operation as it needs some more complex actions. These instructions assume that you have imported the sector template from the ```main-data``` package, as described above. Then, follow these steps:
* Using the Page inxed menu available from the top right drawer, navigate to the ```SectorCode.TemplateSector``` page and make a copy of it page, using the option from the "More actions" menu of this page. Make sure to preserve all children of this page during copy (preserve children box checked)
  * the destination page needs to be in the Sectors location, and needs to have the name of the future sector (like all other sectors). This name will appear in the URL of the sector, you should use a rather short sector name, yet representative. If a longer sector display name is needed, it can be added afterwards in the title of the sector page.
* Setup the rights for this new sector, as follows:
  * go to XWiki.AllSectorResponsiblesGroup and add the SectorResponsiblesGroup of the sector that you just created as a subgroup of this group. E.g. ```Sectors.New Sector.SectorResponsiblesGroup```
  * go to the WebPreferences of the Sector that you just created (e.g. Sectors.New Sector.WebPreferences ). Edit in object mode (prepare URL manually). Find the Global rights object and click the pencil button next to it (on the right) to go in edit mode of this object only. In this form, replace all the occurences of TemplateSector groups (```TemplateSector Sector Responsibles Group```, ```TemplateSector Sector Editors Group``` and ```TemplateSector Initiatives Responsibles Group```) with the respective groups of the new sector you just created (e.g. ```New Sector Sector Responsibles Group```, ```New Sector Sector Editors Group``` and ```New Sector Initiatives Responsibles Group```). Note that when you do this, since the groups suggest only looks in the name of the group page and not in its fullname, you cannot type the sector name to get suggestions, you'll need to type ```InitiativesResponsibles```, ```SectorEditors```, ```SectorResponsibles``` and then choose from all the groups of all the sectors that show up in there.
    * repeat the above operation of replacing rights for the following pages from the subtree of the new sector:
      * Agenda.WebPreferences (e.g. ```Sectors.New Sector.Agenda.WebPreferences```),
      * Blog.WebPreferences (e.g. ```Sectors.New Sector.Blog.WebPreferences```),
      * Initiatives.WebPreferences (e.g. ```Sectors.New Sector.Initiatives.WebPreferences```)
  * delete the ```SectorCode.TemplateSector``` page after this operation (can  be reimported again if an additional sector creation is needed).
