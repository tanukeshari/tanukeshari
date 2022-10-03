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

