# Spotify Domain Model - Glossary

This document provides clear definitions of key terms and components used in the **Spotify Domain Model**.

## 1. User Manager  
A service that handles user authentication, account management, and preferences.  

### **Entities:**
- **System Admin** → Manages users and system settings.  
- **Registered Users** → Individuals who have an account and access Spotify's features.  

### **Operations:**
- Login / Logout  
- Set Preferences  
- Manage User Accounts  

### **Relationships:**
- **System Admin** → manages → **Registered Users**  
- **Registered Users** → connect to → **Playback Manager, Playlist Manager, Subscription & Billing**  

## 2. Playback Manager  
Manages playback controls, session tracking, and queue management.  

### **Entities:**
- **Playback Session** → The current active session where a user is playing audio.  
- **Now Playing Track** → The track currently being played.  
- **Playback Queue** → A list of upcoming tracks in the session.  

### **Operations:**
- Play / Pause / Skip Track  
- Adjust Volume  
- Shuffle / Repeat Mode  

### **Relationships:**
- **Registered Users** → interact with → **Playback Manager**  
- **Playback Manager** → sends listening activity to → **Music Discovery Engine**  

## 3. Playlist Manager  
Enables users to create, edit, and share playlists.  

### **Entities:**
- **User Playlists** → Custom collections of songs created by users.  
- **Songs in Playlist** → Individual tracks added to a playlist.  
- **Collaborators** → Other users who have shared access to a playlist.  

### **Operations:**
- Create / Edit Playlist  
- Add / Remove Songs  
- Share Playlist  

### **Relationships:**
- **Registered Users** → create & manage → **Playlists**  
- **Playlist** → can be shared with → **Collaborators**  
- **Playlist Manager** → notifies → **Notification Manager** (on playlist updates)  

## 4. Music Discovery & Recommendation  
Uses user data to generate personalized music recommendations.  

### **Entities:**
- **Recommendation Engine** → AI-driven system that suggests songs based on listening patterns.  
- **Suggested Songs** → Tracks recommended to the user.  
- **User Listening History** → Data on past songs listened to, used for recommendations.  

### **Operations:**
- Generate Personalized Playlist  
- Suggest New Songs  
- Analyze User Listening Patterns  

### **Relationships:**
- **User Listening Data** → feeds into → **Recommendation Engine**  
- **Recommendation Engine** → provides → **Suggested Songs**  

## 5. Subscription & Billing  
Manages user subscriptions, payment processing, and ad-supported experiences.  

### **Entities:**
- **Subscription Plans (Free / Premium)** → Different tiers of Spotify access.  
- **Payment Processing** → Handles financial transactions for premium users.  
- **Ad Management** → Controls advertisements for free-tier users.  

### **Operations:**
- Process Payments  
- Manage Free & Premium Users  
- Enable / Disable Ads  

### **Relationships:**
- **Registered Users** → subscribe to → **Subscription Plan**  
- **Subscription Plan** → determines → **Ad-Free or Ad-Supported Experience**  
- **Billing System** → processes → **User Payments**  
- **Subscription & Billing** → notifies → **Notification Manager** (on payment updates)  

## 6. Notification Manager  
Sends notifications related to playback, subscriptions, and playlist updates.  

### **Entities:**
- **Playback Notifications** → Alerts for track changes or queue updates.  
- **Subscription Renewal Alerts** → Reminders for expiring subscriptions.  
- **Playlist Updates** → Notifications when playlists are modified.  

### **Operations:**
- Send Track Playback Alerts  
- Notify Subscription Expiry  
- Alert Users on Shared Playlists  

### **Relationships:**
- **User Manager** → receives → **Subscription Alerts** from **Notification Manager**  
- **Playlist Manager** → sends → **Playlist Update Notifications**  
- **Notification Manager** → sends → **Alerts for Playback, Subscription, and Playlists**  