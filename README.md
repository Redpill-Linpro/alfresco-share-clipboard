# Alfresco Share Clipboard

This extension adds a Clipboard to the Alfresco Share document library that allows collecting documents.

Built on Alfresco SDK 4.16.0 and targets **Alfresco Content Services 26.1**.

### Usage

Use the document library action to add files to the clipboard

![Clipboard action in the document libraray](screenshots/action.png)

Files are added to the clipboard view in the left sidebar of the document library

![Clipboard view in the sidebar](screenshots/clipboard-view.png)

An additional clipboard menu can be used to copy, move, link or zip the clipboard contents.

![Clipboard menu in the toolbar](screenshots/clipboard-menu.png)

## Modules

| Module | Contents |
|---|---|
| `alfresco-clipboard-platform` | Repository webscripts (link-to, send-as-mail) and the mail webscript controller |
| `alfresco-clipboard-share` | Share components, Surf customizations, web resources |
| `alfresco-clipboard-platform-docker` | Docker image for the repository, used by the local environment |
| `alfresco-clipboard-share-docker` | Docker image for Share, used by the local environment |
| `alfresco-clipboard-integration-tests` | Integration tests run against the running containers |

## Building

Build with **JDK 21**. Newer JDKs are not supported by ACS 26.

    JAVA_HOME=/usr/lib/jvm/java-21-openjdk mvn clean package

Both modules produce JAR files. To produce AMPs instead, uncomment the
`maven-assembly-plugin` block in the root `pom.xml`.

## Local development environment

The SDK ships a Docker Compose environment covering ACS, Share, Search and PostgreSQL.

    ./run.sh build_start        # build and start everything
    ./run.sh start              # start without rebuilding
    ./run.sh stop               # stop the containers
    ./run.sh reload_share       # rebuild and redeploy the Share module only
    ./run.sh reload_acs         # rebuild and redeploy the platform module only
    ./run.sh tail               # follow the logs
    ./run.sh purge              # remove the volumes and start clean

Once started:

* Alfresco repository — <http://localhost:8080/alfresco>
* Share — <http://localhost:8180/share>

The clipboard module declares `auto-deploy`, so it is active as soon as Share starts.

## Installation

Deploy the two module JARs into an existing installation:

* `alfresco-clipboard-platform/target/alfresco-clipboard-platform-<version>.jar` into the repository webapp's `WEB-INF/lib`
* `alfresco-clipboard-share/target/alfresco-clipboard-share-<version>.jar` into the Share webapp's `WEB-INF/lib`

Restart the webserver afterwards.

## Languages

The Share module ships message bundles for 18 locales — the union of what
Share, the repository and Digital Workspace support:

    ar cs da de en es fi fr it ja nb nl no pl pt_BR ru sv zh_CN

Share's own UI only covers `de en es fr it ja nb nl pt_BR ru zh_CN`, but the
clipboard's strings still resolve for the remaining locales when a user's
browser requests them, so the module is localised even where Share falls back
to English.

Bundles live in two places and both must be kept in sync when adding a key:

| File | Used by |
|---|---|
| `alfresco/web-extension/messages/clipboard*.properties` | Document library actions and the Aikau service |
| `.../site-webscripts/org/alfresco/components/clipboard/menu.get*.properties` | The clipboard menu in the toolbar |

Files are plain ASCII with `\uXXXX` escapes, which is what `java.util.Properties`
expects. The platform module needs no bundles — it produces no user-facing text.

## License

Apache License 2.0 — see [LICENSE](LICENSE) and [NOTICE](NOTICE).

Originally created by Florian Maul (fme AG). This fork is maintained by
[Redpill Linpro](https://www.redpill-linpro.com/).
