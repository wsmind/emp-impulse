# Introduction #

Because the mercurial repositories of the google code project cannot contain big, flat binary files, assets of the project (textures, sounds, ...) are stored on a separate FTP server.

This FTP is supposed to contain various things. Mainly, it will store artistic files (i.e WIP, versioned artwork), as well as exported (downscaled/compiled) data for use in either tests or the final game.

# Folder structure #

The FTP is split in three main directories:
  * **source-assets** is intended to contain everything the artists are working on. Files may be versioned when needed, using a simple version numbering scheme.
  * **test-data** holds data used to test the code. Data here should be similar to what will be found in the final game, but some special assets may also be crafted for testing purposes.
  * **game-data** contains all the assets loaded by the final game

# Workflow considerations #

Everyone (developers, artists) may, at some point, upload data to the FTP. Equally, almost everyone will need to download all or part of it.

## WIPing ##

When an artwork is in its work-in-progress phase, it must be uploaded by artists to the **source-assets** directory. Iterations may be done here, by numbering files or folder when versionning is useful.

## Exporting ##

When an asset is ready to be tried in-game (this doesn't have to wait a final version of the asset), a technically friendly version must be created and put either to **game-data** or **test-data**. Preparing the asset for in-game run might be as simple as copying it to the right directory, but some asset might require special treatment before being loadable by the engine (i.e change of resolution, animation compilation).

## Test data ##

The dedicated folder **test-data** is mainly filled by developers, but artists are free to contribute testing assets ;) The purpose of this directory is essentially to have something to test the code against before any game asset is ready for this.