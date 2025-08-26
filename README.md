# **nomegle ✨ (Senior-Focused Edition)**

nomegle is a simple, anonymous, peer-to-peer video chat application designed with a primary focus on **accessibility and usability for senior citizens**. It connects users randomly for one-on-one conversations in a comfortable and easy-to-navigate environment.

This project is built entirely with frontend technologies, using Firebase's Firestore as a serverless backend for signaling (the process of connecting two users).

## **👴 A Focus on Senior Usability**

This application was enhanced based on user experience research, including the systematic review **"Optimizing mobile app design for older adults"** and usability studies like the one from **Toss.tech** (https://toss.tech/article/senior-usability-research). Our goal is to bridge the digital divide by creating an inclusive space that addresses the common challenges older adults face with technology.

### **Key Accessibility & Usability Features:**

* **Welcome Guide:** A clear, large-text modal appears on the first visit to explain the app's simple functions, providing the **initial training** recommended by research.  
* **Accessibility Controls:** A dedicated menu (♿) allows users to:  
  * **Increase Text Size:** Cycle through multiple font sizes to improve readability.  
  * **Enable High-Contrast Mode:** Switch to a high-contrast theme for better visibility.  
* **Simplified Interface:**  
  * **Large, Clear Buttons:** All interactive elements are large, well-spaced, and clearly labeled to minimize errors from motor skill impairments.  
  * **Minimalist Design:** The interface avoids clutter and complex navigation paths, maintaining a clear focus on the current action, a core principle of senior-friendly design.  
* **Enhanced Safety:** An "End & Report" button provides users with a greater sense of security and control.

## **🚀 Setup and Deployment**

To get your own version of nomegle running, follow these steps:

### **1\. Create a Firebase Project**

1. Go to the [Firebase Console](https://console.firebase.google.com/) and click **"Add project"**.  
2. Give your project a name (e.g., "my-nomegle-app") and follow the on-screen instructions.

### **2\. Configure Firebase Services**

1. **Enable Authentication:**  
   * In your Firebase project, go to **Build \> Authentication**.  
   * Click **"Get started"**.  
   * Select the **"Sign-in method"** tab.  
   * Choose **"Anonymous"** from the list, enable it, and click **Save**.  
2. **Create a Firestore Database:**  
   * Go to **Build \> Firestore Database**.  
   * Click **"Create database"**.  
   * Select **"Start in test mode"**.  
   * Choose a location for your database and click **Enable**.  
3. **Update Firestore Security Rules:**  
   * In the Firestore Database section, click the **"Rules"** tab.  
   * Replace the existing rules with the following:  
     rules\_version \= '2';  
     service cloud.firestore {  
       match /databases/{database}/documents {  
         match /{document=\*\*} {  
           allow read, write: if true;  
         }  
       }  
     }

   * Click **"Publish"**.

### **3\. Configure Your HTML File**

1. **Get Your Firebase Config:**  
   * Go to your **Project Settings** (click the gear icon ⚙️).  
   * In the "Your apps" card, click the web icon (\</\>).  
   * Register your app and Firebase will provide a firebaseConfig object.  
2. **Paste the Config:**  
   * Open the index.html file.  
   * Find the section marked // \--- PASTE YOUR FIREBASE CONFIG HERE \---.  
   * Replace the placeholder object with the firebaseConfig object you just copied.

### **4\. Deploy with Firebase Hosting**

1. **Install Firebase Tools:** If you haven't already, open a terminal and run:  
   npm install \-g firebase-tools

2. **Login and Initialize:**  
   * Run firebase login to sign in.  
   * Navigate to your project directory in the terminal.  
   * Run firebase init hosting.  
   * Select **"Use an existing project"** and choose the project you created.  
   * When asked for the public directory, enter . (a single period).  
   * Answer **No** to configuring as a single-page app and overwriting index.html.  
3. **Deploy:**  
   * Run the final command:  
     firebase deploy

   * Firebase will give you a public Hosting URL. Your nomegle app is now live\!

## **How It Works**

1. **User A Clicks "Start a Chat":** The app accesses their camera and creates a new "room" document in Firestore. This document contains a WebRTC "offer" and a waiting: true status.  
2. **User B Clicks "Start a Chat":** The app searches Firestore for a room where waiting: true. It finds User A's room.  
3. **The Handshake:**  
   * User B reads User A's "offer" and generates an "answer".  
   * User B updates the room document with their "answer" and sets waiting: false.  
   * Both users listen for changes to the room and exchange network candidates (ICE candidates) through Firestore subcollections.  
4. **Direct Connection:** Once enough information has been exchanged, WebRTC establishes a direct, peer-to-peer connection between User A and User B. Firestore is no longer needed for this chat session.  
5. **Ending a Chat:** When a user clicks "Next" or "End", the app closes the connection and deletes the room document from Firestore, making it available for the next users.