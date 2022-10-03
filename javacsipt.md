- import { FileUploadHandler, FileUploadManager } from '@azure/communication-react';

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

