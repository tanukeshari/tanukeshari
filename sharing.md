- File Sharing




import { FileUploadHandler, FileUploadManager } from '@azure/communication-react';

import { initializeFileTypeIcons } from '@fluentui/react-file-type-icons'; 

import {

 ChatComposite,

 fromFlatCommunicationIdentifier,

 useAzureCommunicationChatAdapter

} from '@azure/communication-react';

import React, { useMemo } from 'react';



initializeFileTypeIcons();



function App(): JSX.Element {

 // Common variables

 const endpointUrl = 'INSERT_ENDPOINT_URL';

 const userId = ' INSERT_USER_ID';

 const displayName = 'INSERT_DISPLAY_NAME';

 const token = 'INSERT_ACCESS_TOKEN';

 const threadId = 'INSERT_THREAD_ID';



 // We can't even initialize the Chat and Call adapters without a well-formed token.

 const credential = useMemo(() => {

  try {

   return new AzureCommunicationTokenCredential(token);

  } catch {

   console.error('Failed to construct token credential');

   return undefined;

  }

 }, [token]);



 // Memoize arguments to `useAzureCommunicationChatAdapter` so that

 // a new adapter is only created when an argument changes.

 const chatAdapterArgs = useMemo(

  () => ({

   endpoint: endpointUrl,

   userId: fromFlatCommunicationIdentifier(userId) as CommunicationUserIdentifier,

   displayName,

   credential,

   threadId

  }),

  [userId, displayName, credential, threadId]

 );

 const chatAdapter = useAzureCommunicationChatAdapter(chatAdapterArgs);



 if (!!chatAdapter) {

  return (

   <>

    <div style={containerStyle}>

     <ChatComposite

      adapter={chatAdapter}

      options={{

       fileSharing: {

        uploadHandler: fileUploadHandler,

        // If `fileDownloadHandler` is not provided. The file URL is opened in a new tab.

        downloadHandler: fileDownloadHandler,

        accept: 'image/png, image/jpeg, text/plain, .docx',

        multiple: true

       }

      }} />

    </div>

   </>

  );

 }

 if (credential === undefined) {

  return <h3>Failed to construct credential. Provided token is malformed.</h3>;

 }

 return <h3>Initializing...</h3>;

}



const fileUploadHandler: FileUploadHandler = async (userId, fileUploads) => {

 for (const fileUpload of fileUploads) {

  try {

   const { name, url, extension } = await uploadFileToAzureBlob(fileUpload);

   fileUpload.notifyUploadCompleted({ name, extension, url });

  } catch (error) {

   if (error instanceof Error) {

    fileUpload.notifyUploadFailed(error.message);

   }

  }

 }

}









upload method


const uploadFileToAzureBlob = async (fileUpload: FileUploadManager) => {

 // You need to handle the file upload here and upload it to Azure Blob Storage.

 // Below you can find snippets for how to configure the upload

 // Optionally, you can also update the file upload progress.

 fileUpload.notifyUploadProgressChanged(0.2);

 return {

  name: 'SampleFile.jpg', // File name displayed during download

  url: 'https://sample.com/sample.jpg', // Download URL of the file.

  extension: 'jpeg' // File extension used for file icon during download.

 };

  

const fileDownloadHandler: FileDownloadHandler = async (userId, fileData) => {

   return new URL(fileData.url);

  }

 };

}







const uploadFileToAzureBlob = async (fileUpload: FileUploadManager) => {

 const file = fileUpload.file;

 if (!file) {

  throw new Error('fileUpload.file is undefined');

 }



 const filename = file.name;

 const fileExtension = file.name.split('.').pop();



 // Following is an example of calling an Azure Function to handle file upload

 // The https://learn.microsoft.com/azure/developer/javascript/how-to/with-web-app/azure-function-file-upload

 // tutorial uses 'username' parameter to specify the storage container name.

 // Note that the container in the tutorial is private by default. To get default downloads working in

 // this sample, you need to change the container's access level to Public via Azure Portal.

 const username = 'ui-library';

  

 // You can get function url from the Azure Portal:

 const azFunctionBaseUri='<YOUR_AZURE_FUNCTION_URL>';

 const uri = `${azFunctionBaseUri}&username=${username}&filename=${filename}`;

  

 const formData = new FormData();

 formData.append(file.name, file);

  

 const response = await axios.request({

  method: "post",

  url: uri,

  data: formData,

  onUploadProgress: (p) => {

   // Optionally, you can update the file upload progess.

   fileUpload.notifyUploadProgressChanged(p.loaded / p.total);

  },

 });

  

 const storageBaseUrl = 'https://<YOUR_STORAGE_ACCOUNT>.blob.core.windows.net';



 return {

  name: filename,

  url: `${storageBaseUrl}/${username}/${filename}`,

  extension: fileExtension

 };

}







import { FileUploadHandler } from from '@azure/communication-react';



const fileUploadHandler: FileUploadHandler = async (userId, fileUploads) => {

 for (const fileUpload of fileUploads) {

  if (fileUpload.file && fileUpload.file.size > 99 * 1024 * 1024) {

   // Notify ChatComposite about upload failure. 

   // Allows you to provide a custom error message.

   fileUpload.notifyUploadFailed('File too big. Select a file under 99 MB.');

  }

 }

}





File Downloads 
import { FileDownloadHandler } from "communication-react";



const isUnauthorizedUser = (userId: string): boolean => {

 // You need to write your own logic here for this example.

}



const fileDownloadHandler: FileDownloadHandler = async (userId, fileData) => {

 if (isUnauthorizedUser(userId)) {

  // Error message will be displayed to the user.

  return { errorMessage: 'You don’t have permission to download this file.' };

 } else {

  // If this function returns a Promise that resolves a URL string, 

  // the URL is opened in a new tab. 

  return new URL(fileData.url);

 }

}
