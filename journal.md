# Journal

## 11242020

Wrote the [kickoff post](https://jasongullickson.com/osw-operating-system-for-the-web.html) for OSW.  Still noodling on exactly where to get started, but probably with writing a spec for the API, ideally something that can be used to create some automated tests (perhaps [openapi](https://learn.openapis.org/)?) so we can do this in proper TDD-style, but without dragging-in a bunch of tooling and dependencies.  I really want this to be simple and fun to work on and I don't want a bunch of stuff that makes development a headache.

There's a few things I didn't write about in the kickoff post that I'll add here just so I don't loose track of them.  The first that comes to mind is the use of `localhost` to address resources on the underlying host O/S or hardware.  One example of this is the GPIO pins of a single-board computer.  By only allowing direct access to this via paths under `localhost` (i.e.: `http://localhost/gpio/1`) only code running as an X can access these resources in "production" while allowing direct access to them in development (working against a local OSW instance).

I also touch on the idea of replacing the `private` parameter with a convention.  I don't have a specific idea for this yet, but what I'm thinking here is a path convention where items under one path are always public and items under another path are always private.  This could be a split in the tree of a path or perhaps a naming convention for a pathname, but the objective is to a) simplify the API and b) make it harder to publish something publicly by accident w/o making everyone just always use `private` by default.  Public data has many pro-social advantages so we don't want to scare people away from using it, but we also don't want to make it easy to make mistakes.

I also mention executables (X) and federation.  X will probably work a lot like the [executable experiments](https://github.com/jjg/jsfs/blob/jsfsx/docs/jsfsx.md) I did fairly recently in JSFS.  Javascript files uploaded with the X flag set are executed by Node.js in a VM.  I'm still not crystal clear as to if this runs as a [worker thread](https://nodejs.org/api/worker_threads.html) or something else, but that will be the goal.

Federation is a little less defined.  There have been a number of experiments with federation from simple static lists of federated hosts to dynamic distributed hashtables and even blockchains, but regardless of the network implementation the two essential components are the exchange of inodes and the ability to distribute blocks.  For the first pass I'm tempted to pick something simple and safe and isolate the federation networking so that we can try a few things with minimal impact at the API level.  I left-out any discussion of "pluggable" back-ends (the ability to easily switch from say storing blocks on local disks to storing them in say S3) but isolating the federation code at the same level might make a lot of sense.

Enough chit-chat, let's make something.

### API

#### Headers/parameters
These values can be specified as either [HTTP Headers]() or [URL Parameters]().  If specified as headers, terms must be separated by `-`, if specified as parameters, separators must be `_`.  Parameters must omit any leading `x-` notation which is required for non-standard HTTP headers.

`Authorization`
This header is required for all requests to a `private` resource.


