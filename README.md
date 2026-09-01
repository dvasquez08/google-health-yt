# Welcome

This is where you will find the content related to my video for connecting the Google Health API with Claude Code. The other files in the repo are the prompts that I used in my video. Below are the steps that I took for the setup, and some of the things that you will need to paste into Google Cloud Console, and in your project files.

## Part 1: Google Cloud Console

- In the video I show you how to setup your OAuth Consent Screen for your project in your Google Cloud console. These are the things that you will need for that setup.

- Call back URI: http://localhost:3000/auth/callback
- Scope: https://www.googleapis.com/auth/googlehealth.activity_and_fitness.readonly

## Part 2: Project Folder Prep

Create a new folder for your project files, and to work out of in Claude Code. Create a text file and call it .env. Past the following in, replacing the placeholders with your Google account info.

```env
GOOGLE_CLIENT_ID=your-client-id
GOOGLE_CLIENT_SECRET=your-client-secret
GOOGLE_REDIRECT_URI=http://localhost:3000/auth/callback
PORT=3000
```

## Part 3: Project Build

Time to build your project! Copy the prompts in the markdown files in this repo, in the nightly reporting prompt, ensure to replace the placeholders with your location and time zone info. 

[Check out the other videos in my channel!](https://www.youtube.com/@davtekio)
