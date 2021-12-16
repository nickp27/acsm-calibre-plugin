# Calibre ACSM Input plugin

This is a Calibre plugin that allows you to turn ACSM files into EPUB or PDF files without the need for Adobe Digital Editions. 
It is a full Python reimplementation of libgourou by Grégory Soutadé (http://indefero.soutade.fr/p/libgourou/).

## Setup

Download the plugin and import it into Calibre, then open the plugin settings. The plugin should display "Not authorized for any ADE ID". You now have multiple options to authorize the plugin: 

- You can click on "Link to ADE account" and enter your AdobeID credentials to link your Calibre installation to your AdobeID account. This uses up one of your available activations. 
- You can click on "Create anonymous authorization" to create an anonymous authorization. Make sure to create backups of that authorization. Also, if you do end up emulating ADE3+ and you receive a file with the new Adobe DRM, you might not be able to access it ...
- If you have ADE installed and activated on your machine, you can click "Import activation from ADE" to clone the existing activation from your ADE installation. 
- If you have used this plugin before, you can click on "Import existing activation backup" to import a previously created activation backup (ZIP) file to restore an activation. This functionality can also be used to clone one activation to multiple computers. 

During authorization, the plugin may ask you for the ADE version to emulate. Usually you can leave this setting as it is (ADE 2.0.1).

After you've activated the plugin, make a backup of the activation using the "Export account activation data". Then click "Export account encryption key" and import the resulting file into the DeDRM plugin for DRM removal. If you're using noDRM's fork of the DeDRM plugin, this step will happen automatically. 

Once that's done, download an ACSM file from Adobe's test library and see if you can import it into Calibre: https://www.adobe.com/de/solutions/ebook/digital-editions/sample-ebook-library.html 

IMPORTANT: 

- I would suggest creating a new dummy AdobeID to use for Calibre so just in case Adobe detects this and bans you, you don't lose your main AdobeID. 
- Combined with that I suggest using the DeDRM plugin to make sure that losing your AdobeID doesn't also mean you'll lose access to all your eBooks. 
- If you use an anonymous authorization, make sure you make backups of the activation data. 
- If you use an anonymous authorization, you have the ability to copy that authorization into an AdobeID account at a later time (by clicking "Connect anonymous auth to ADE account"). This is useful if you have books linked to your authorization that you want to read elsewhere. Same restrictions as with ADE apply - you can only do this ONCE per AdobeID, and only if the AdobeID hasn't been in use elsewhere yet.
- This software is not approved by Adobe. I am not responsible if Adobe detects that you're using nonstandard software and bans your account. Do not complain to me if Adobe bans your main ADE account - you have been warned. 

## Returning books

If a book is marked as returnable (like a library book), you can "return" it to the library using this plugin. 
Just open the plugin settings, click "Show loaned books" (the option is only visible if you have at least one loaned book that's been downloaded with this plugin), select the book, then click the arrow button to return. Or click the "X" button to just remove the loan record from the list without returning the book.

This makes the book available for someone else again, but it does not automatically get deleted from your Calibre library - you are responsible for doing that after returning a book.

Note: You can only return books that you downloaded with version 0.0.9 (or newer) of this plugin. You cannot return books downloaded with ADE or with earlier versions of this plugin.

## Standalone version

In the folder "calibre-plugin" in this repo (or inside the Calibre plugin ZIP file) there's some scripts that can also be used standalone without Calibre. If you want to use these, you need to extract the whole ZIP file. 

- `register_ADE_account.py` can be used to authorize a computer with an ADE account. This creates the three files `activation.xml`, `device.xml` and `devicesalt`. These files are in the same format as the ones for the Calibre computer authorization (inside the `plugins/DeACSM/account/` folder). A ZIP activation export from Calibre will also contain these three files.
- `fulfill.py` can be used - with valid ADE account files in the same folder - to turn an URLLink.acsm file into an eBook. 
- `get_key_from_Adobe.py` can be used to contact the Adobe server and download the DER key file for a given AdobeID to import into a DeDRM plugin. Just input your credentials and this script will output the DER file that can be used with DeDRM to decrypt books for this AdobeID. This works independantly from the account activation files or from this plugin, so it's a good way to get your AdobeID decryption key, especially if you're on Linux and it's too difficult to export the keys from ADE running inside Wine. This process does not use up one of your six device activations.

Though, generally it's recommended to use the Calibre plugin instead of these standalone scripts. Except for maybe the `get_key_from_Adobe.py` script if you want to remove DRM from existing eBooks without having to extract the key from ADE.

## To-Do list for the future?

- Support to copy an authorization from the plugin to an ADE install
- Support for Adobe's "auth" download method instead of the "simple" method.
- Support to authorize an eReader that's connected over USB
- ...