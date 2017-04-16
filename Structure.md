Explanations of the structure of files and folders within Theos:

* **bin/**: Contains various scripts used both internally by Theos and externally by the user, as well as the implementations of [[Logos]] and [[NIC]].
* **extras/**: Extra, optional, goodies bundled with Theos.
  * **vim/**: Logos syntax files for use with vim and similar editors.
* **include/**: Provided to place headers in.
* **lib/**: Provided to place libraries and frameworks in.
* **makefiles/**: The makefiles that comprise the majority of Theos itself.
  * **install/**: Rules for installing packages on different platforms. These execute commands upon install and uninstall of the package.
  * **instance/**: Makefiles included from a [sub-`make`](https://www.gnu.org/software/make/manual/html_node/Recursion.html) when building an individual instance (project). This includes the compilation of source.
  * **master/**: Makefiles included from the master `make` invocation.
  * **package/**: Rules for building packages for various packaging formats. These execute commands upon building of the package.
  * **platform/**: Makefiles included depending on the current operating system platform. These set up the Theos environment appropriately for the platform.
  * **targets/**: Makefiles included depending on the current operating system platform, and the platform being targeted. These set up the Theos environment appropriately to build for a platform.
* **mod/**: Provided to place modules in. Theos will automatically include various files from here.
* **sdks/**: Provided to place SDKs in.
* **templates/**: Built-in and user-created [[NIC]] templates.
* **toolchain/**: Provided to place toolchains in, as directed at [[Installation]].
* **vendor/**: Submodule components included with Theos.
  * **include/**: [Built-in headers](https://github.com/theos/headers) that may or may not be useful for most projects.
  * **lib/**: [Built-in library](https://github.com/theos/lib) definitions that may or may not be useful for most projects.
* **Prefix.pch**: The prefix header imported into the compilation process for all C-based languages. Used to define macros and import framework headers.