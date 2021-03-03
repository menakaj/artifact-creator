# artifact-creator
Prerequisite: Node 12.x ( will not work with 14, 10 etc.. ) Use NVM to install node 12.

This helps to create support artifacts for the update-artifacts repo and provide an easy way to add them during the PR Analysis phase.

This script will generate the following.

```
The artifacts folder
run-in-pmt.js
```

1. Get the latest live U2 updated product.
2. Merge all support PRs
3. Trigger the Jenkins build.
4. Download each feature builds from the Jenkins after the build is successful.
5. Clone or download the gen.js from this repo.
6. Extract all the above zip files ( product, feature archives ) at the same location.

Example.

```bash
.
├── gen.js
├── org.wso2.carbon.apimgt.admin.feature-6.7.206
│   ├── features
│   └── plugins
├── org.wso2.carbon.apimgt.store.feature-6.7.206
│   ├── features
│   └── plugins
└── wso2am-3.2.0
    ├── INSTALL.txt
    ├── LICENSE.txt
    ├── README.txt
    ├── XMLInputFactory.properties
    ├── bin
    ├── business-processes
    ├── dbscripts
    ├── lib
    ├── modules
    ├── release-notes.html
    ├── repository
    ├── resources
    ├── samples
    ├── tmp
    ├── updates
    └── wso2update_linux
```

- Note1: The folder structure is important.
- Note2: The gen.js copied at the same location.

Open the gen.js and update the following section according to the patch number and product version.

```js
// ******************************************************** //
// ******************************************************** //
// ******************************************************** //
//          This part need to modified accordingly          //
const jars = [
   // {
   //     jarName: 'org.wso2.carbon.apimgt.publisher.feature-6.7.206',
   //     appContext: 'publisher',
   // },
   {
       jarName: 'org.wso2.carbon.apimgt.store.feature-6.7.206',
       appContext: 'devportal',
   },
   {
       jarName: 'org.wso2.carbon.apimgt.admin.feature-6.7.206',
       appContext: 'admin',
   }
]
 
 
const productName = 'wso2am-3.2.0';
const artifactFolderName = '0551';
 
 
// ******************************************************** //
// ******************************************************** //
// ******************************************************** //
```

Note: You need to comment out the unwanted webapps.

Open a terminal and run the following command.

```bash
node gen.js
```

The following output will be shown.

```bash
/folderpath/0551/devportal/site/public/dist  created 
/folderpath/0551/admin/site/public/pages  created 
7 files to added to the devportal dist folder
7 files removed from the old pack's devportal dist folder
/folderpath/0551/admin/site/public/dist  created 
4 files to added to the admin dist folder
4 files removed from the old pack's admin dist folder
run-in-pmt.js file is saved
```

In U2, you have to upload the modified/ added files. If there are multiple files, you can upload them as an archive.

Goto the artefacts folder and archive each directory.

```bash
zip -r publisher.zip publisher
```


Next, you need to go to the “PR analysis" do the following.

1. To upload the added files, click on ```+ Manual File```.
2. Provide the path of the pack where the fils should be copied. 
Eg. repository/deployment/server/jaggeryapps
3. Selected Added from the dropdowns list.
4. Select the required zip file to be uploaded and click Add.

Then the zip file will be uploaded/ extracted and list all the files that needs to be added.

To add the removed files, run the run-in-umt.js. To run the script you need to open the developer tools “Console” tab and paste the content and press “Return/Enter”

## Note.
Since there are several files to be added and removed, better to use separate updates for each app. (update per each ui app)
