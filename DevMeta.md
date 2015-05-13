This page describes how is organized this wiki and what simple rules apply in order to keep it clean.

# Virtual namespacing #
The Google Code wiki doesn't support namespacing, so we directly prefix page names with a namespace name. For example, this page is in the developer namespace ("Dev"), and is thus called DevMeta. Note that, to avoid very long names, the names are prefixed only with the last namespace they belong to. For instance, a page within the Engine > Graphics namespace will be prefixed only by "Graphics". Thus all namespace names must be distinct, even if they belong to different parent namespaces.

The page hierarchy is reflected by the SideMenu page, a special page containing only embedded lists and being displayed as the menu you see on the left.

# Page labels #
Labels are meta-tags associated with a page and allow the writer to give information about its page. Currently, the following labels are available :

  * Featured             = Listed on project home page (use carefully)
  * WorkInProgress       = The content is being worked on (remove when the content is finalized)
  * Deprecated           = Most users should NOT reference this

Generally, a new page will be tagged WorkInProgress until its content is really OK.

# Comments #

Don't hesitate to post comments on the different pages, in order to notify the author that the content must be updated or changed. You may also add the WorkInProgress label, if you think it's worth it.

# Wiki syntax #
The wiki help can be found [there](http://code.google.com/p/support/wiki/WikiSyntax) (also linked in page edition mode, in the blue box on the right side).

# Image upload #
Inserting images is very easy (just put a link to an image file), but to upload it, you must access the wiki through [its dedicated Hg repository](http://code.google.com/p/emp-impulse/source/checkout?repo=wiki) and add your image into the images/ directory.
Alternatively, you can use the free [yuml.me](http://yuml.me) service for UML diagrams, and published Google Docs drawings for other type of diagrams.