---
title: "Authentication with AWS Amplify"
date: 2023-06-09T19:38:13-07:00
draft: false
categories:
  - Software Development
  - Technology
tags:
  - AWS Amplify
  - Authentication
  - React Native
  - Cutomized Authentication
---

# Authentication with AWS Amplify

User authentication is a fundamental requirement for many mobile applications. AWS Amplify provides a comprehensive solution for implementing user authentication in React Native apps. In this blog post, we will explore how to leverage Amplify Authentication to secure your React Native app with user sign-up, sign-in, and user management functionalities.

## 1- What is AWS Amplify Authentication?

AWS Amplify Authentication is a comprehensive authentication service offered by AWS Amplify, designed to simplify the process of implementing secure user authentication in web and mobile applications. It provides a set of features and functionalities that enable developers to handle user authentication seamlessly, including user sign-up, sign-in, password recovery, and user management.

## 2- Prerequisites

To implement Amplify Authentication in your React Native app, there are a few prerequisites you should have in place. Here is a list of the common prerequisites:

- Node.js and npm:
Ensure that you have Node.js (version 12 or higher) and npm (version 6 or higher) installed on your development machine. Node.js and npm are essential for managing packages and running JavaScript applications.

- AWS Account:
You need to have an AWS account to use Amplify Authentication. If you don't have an AWS account, you can create one at the AWS Management Console (https://aws.amazon.com/console/).

- AWS CLI:
Install the AWS Command Line Interface (CLI) on your machine. The AWS CLI allows you to interact with various AWS services from the command line. You can install the AWS CLI by following the instructions provided in the official AWS CLI documentation (https://aws.amazon.com/cli/).

- Amplify CLI:
Install the Amplify Command Line Interface (CLI) globally on your machine. The Amplify CLI is used to configure and manage your Amplify project. You can install it by running the following command:

```
npm install -g @aws-amplify/cli
```
- Amplify CLI Configuration:
Configure the Amplify CLI by running the following command and following the prompts:
```
amplify configure
```
This step involves providing your AWS access keys, setting up a default region, and selecting a user profile to associate with Amplify.

- React Native Project:
Set up a React Native project using the React Native CLI or a tool like Expo. Make sure your project is initialized and ready for development.

## 3- Setting Up AWS Amplify
- Initialize Amplify in your React Native project
Navigate to the root directory of your React Native project using the terminal. Run the following command to initialize Amplify:
```
amplify init
```

This command initializes Amplify in your project and prompts you to answer a series of questions to configure the Amplify project settings. Choose the default options or customize them according to your project requirements.

- Add Amplify Authentication
To add Amplify Authentication to your project, run the following command in the terminal:
```
amplify add auth
```
The Amplify CLI will guide you through a series of prompts to configure the authentication settings. Choose the options that suit your application's needs, such as the authentication type (username/password, social sign-in, etc.), multi-factor authentication, and password policy.

- Push the changes to AWS
Once you have configured Amplify Authentication, run the following command to push the changes to your AWS account:
```
amplify push
```
The Amplify CLI will provision the necessary AWS resources, such as Amazon Cognito for authentication, and configure them based on your project settings.

After completing these steps, your React Native app will be set up with Amplify Authentication, and you can start using the authentication features provided by Amplify in your app.

## 4- Adding Authentication to Your React Native App

Configure Amplify in your React Native app
In your React Native project, open the index.js or App.js file and add the following code at the top to configure Amplify:
```
import { withAuthenticator, AmplifyTheme } from 'aws-amplify-react-native';
import {Auth} from 'aws-amplify';
import Amplify from 'aws-amplify';
```
Below is a sample piece of code that you can use to get the currently authenticated user and
if it does not exist you can exit:
```
  // get currently authenticated user
  const userInfo = await Auth.currentAuthenticatedUser({bypassCache: true});
  if (!userInfo) {
    return;
  }
```

## 5- Customize user authentication

You may want to modify the authentication form look and feel or define what fields
you want to collect from users. The snippet below
```
const MySectionHeader = Object.assign({}, AmplifyTheme.sectionHeader,
  { background: 'orange' });
const MyInputLabel = Object.assign({}, AmplifyTheme.inputLabel,
  { background: 'black', color: 'black' });
const MyErrorText = Object.assign({}, AmplifyTheme.errorRowText,
  { background: 'red', color: 'red' });

const MyTheme = Object.assign({}, AmplifyTheme,
  { sectionHeader: MySectionHeader, inputLabel: MyInputLabel,
    errorRowText: MyErrorText });

const signUpConfig = {
  header: 'Register new user',
  hideAllDefaults: true,
  defaultCountryCode: '1',
  signUpFields: [
    {
      label: 'first name',
      key: 'given_name',
      required: true,
      displayOrder: 1,
      type: 'string'
    },
    {
      label: 'last name',
      key: 'family_name',
      required: true,
      displayOrder: 2,
      type: 'string'
    },
    {
      label: 'email address',
      key: 'email',
      required: true,
      displayOrder: 3,
      type: 'string'
    },
    {
      label: 'Password',
      key: 'password',
      required: true,
      displayOrder: 4,
      type: 'string'
    },
    {
      label: 'zipcode (5 digits)',
      key: 'custom:zipcode',
      required: true,
      displayOrder: 5,
      type: 'string'
    },
    {
      label: 'city',
      key: 'custom:city',
      required: true,
      displayOrder: 6,
      type: 'string'
    },
    {
      label: 'state (2 letters)',
      key: 'custom:state',
      required: true,
      displayOrder: 7,
      type: 'string'
    },
    {
      label: 'country',
      key: 'custom:country',
      required: true,
      displayOrder: 8,
      type: 'string'
    }
  ]
};

const usernameAttributes = 'email address';

export default withAuthenticator(App, {signUpConfig, usernameAttributes},
  [], null, MyTheme);
```
