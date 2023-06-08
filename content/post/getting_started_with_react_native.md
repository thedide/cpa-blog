---
title: "Getting started with react native"
date: 2023-06-03T00:20:53Z
draft: true
categories:
  - Software Development
  - Technology
tags:
  - App development
  - React Native
---

# Getting started with React Native
To initialize a React Native project, build a "Hello World" app, and run it, you can follow these steps:

1. Install Required Tools
Before getting started, make sure you have Node.js and npm (Node Package Manager) installed on your machine. You'll also need a code editor, such as Visual Studio Code, to write your React Native app.

2. Install React Native CLI
Open your terminal or command prompt and run the following command to install the React Native CLI globally:

```
npm install -g react-native-cli
```

3. Create a New React Native Project
In your terminal or command prompt, navigate to the directory where you want to create your React Native project. Run the following command to create a new project:

```
react-native init HelloWorld
```
or in case you use npx:
```
npx react-native init HelloWorld
```

This command will create a new directory called "HelloWorld" and set up the basic structure for your React Native app.

4. Navigate to Project Directory
Navigate into the project directory using the following command:

```
cd HelloWorld
```

5. Build and Run the App

To build and run your app on a connected device or emulator, run the following command:

For iOS:

```
react-native run-ios
```

For Android:

```
react-native run-android
```

You can add 'npx react-native run-ios/run-android' if you use npx.

This command will compile your app and launch it on the iOS Simulator (for iOS) or an Android emulator (for Android). Make sure you have a simulator/emulator set up or a physical device connected to your machine.

6. Update App.js with "Hello World" Text
Open the App.js file in your preferred code editor. Replace the existing code with the following:

```
import React from 'react';
import { View, Text } from 'react-native';

const App = () => {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Hello World!</Text>
    </View>
  );
};

export default App;
```

7. Save and Refresh
Save the App.js file, and your app will automatically refresh in the simulator/emulator with the updated "Hello World" text.

Congratulations! You have successfully created a React Native project, built a "Hello World" app, and run it on a simulator/emulator. You can now start exploring React Native and building more complex applications.

8. Useful packages
Here are a few useful packages that you can add to your package.json and use them in your apps:

1. @react-native-async-storage/async-storage: Provides an asynchronous, persistent, key-value storage system for React Native apps.

2. @react-native-community/masked-view: Offers a masked view component for React Native, allowing you to create visually interesting masks for UI elements.

3. @react-native-community/netinfo: Allows you to monitor network connectivity and retrieve information about the device's network state.

4. @react-native-picker/picker: Provides a cross-platform picker component for selecting items from a dropdown list.

5. @react-navigation/bottom-tabs: Enables the creation of a tab-based navigation interface at the bottom of the screen using React Navigation.

6. @react-navigation/native: Provides the core navigation functionality for React Native apps, allowing you to navigate between screens and manage the navigation stack.

7. @react-navigation/stack: Offers a stack-based navigation system using React Navigation, allowing you to manage a stack of screens and navigate between them.

8. @rneui/themed: A library that provides theming capabilities for React Native components, allowing you to customize the appearance of your app easily.

9. amazon-cognito-identity-js: A JavaScript SDK for working with Amazon Cognito, which provides user authentication and authorization services.

10. aws-amplify: A library that simplifies the integration of AWS services into your app, including authentication, storage, and APIs.

11. aws-amplify-react-native: Provides React Native-specific components and utilities for working with AWS Amplify.

12. react: The core React library that enables the creation of reusable UI components.

13. react-native: The framework that allows you to build mobile apps using JavaScript and React, targeting both iOS and Android platforms.

14. react-native-gesture-handler: Provides gesture recognition and handling capabilities for React Native apps, enabling touch-based interactions.

15. react-native-get-random-values: A polyfill for the crypto.getRandomValues() function, which allows you to generate cryptographically secure random values.

16. react-native-image-picker: Enables the selection of images and videos from the device's gallery or camera in React Native apps.

17. react-native-keyboard-aware-scroll-view: A scroll view component that automatically adjusts its height to accommodate the keyboard and ensure content remains visible when the keyboard is active.

18. react-native-reanimated: A library for building smooth and performant animations in React Native apps, leveraging the Animated API.

19. react-native-safe-area-context: Provides a way to handle safe areas on iOS and Android devices, ensuring content is correctly displayed within device boundaries.

20. react-native-screens: Offers a cross-platform API for managing native screens in React Native, improving performance and memory efficiency.

21. react-native-tab-navigator: Deprecated package. Previously used for creating tab-based navigation in React Native apps, but it's recommended to use other navigation solutions, like React Navigation.

22. react-native-vector-icons: Allows you to use vector icons in your React Native app, providing a wide range of customizable icons.

23. react-native-video: Enables the playback of videos in React Native apps, supporting various formats and customization options.

24. uuid: A library for generating and working with Universally Unique Identifiers (UUIDs), which are often used for unique key generation and data identification.

9. Organizing your code
Organizing your React Native code into components, navigation, and screens is a common practice that helps maintain a clean and structured codebase. Here's a suggested approach for organizing your code:

1. *Components Folder*: Create a folder named "components" at the root of your project. This folder will contain reusable UI components that can be used across different screens. Each component should have its own folder with a JavaScript file for the component's logic and a style file for its styles.

For example:
```
components/
├── Button/
│   ├── Button.js
│   └── ButtonStyles.js
├── TextInput/
│   ├── TextInput.js
│   └── TextInputStyles.js
└── ...
```

2. *Navigation Folder*: Create a folder named "navigation" at the root of your project. This folder will contain all the navigation-related code, including navigators, screens, and navigation configuration files.

For example:
```
navigation/
├── MainNavigator.js
├── HomeScreen.js
├── ProfileScreen.js
└── ...

```

In MainNavigator.js, you can define the main navigation stack or bottom tab navigator using a library like React Navigation. Each screen file (HomeScreen.js, ProfileScreen.js, etc.) represents a different screen of your app.

3. *Screens Folder*:Create a folder named "screens" at the root of your project. This folder will contain the main screens of your app, which are responsible for rendering the UI and handling user interactions. Each screen should have its own folder with a JavaScript file for the screen's logic and a style file for its styles.

For example:
```
screens/
├── Home/
│   ├── HomeScreen.js
│   └── HomeScreenStyles.js
├── Profile/
│   ├── ProfileScreen.js
│   └── ProfileScreenStyles.js
└── ...

```

4. *Shared Folder*: If you have any shared code or utilities that are not specific to a particular component, navigation, or screen, you can create a "shared" folder at the root of your project. This folder can contain files such as utility functions, constants, or context providers that can be accessed by different parts of your app.

For example: 
```
shared/
├── utils.js
├── constants.js
└── MyContext.js

```

By organizing your code in this way, you can easily locate and manage different parts of your app. It promotes reusability, readability, and maintainability, making it easier for you and your team to collaborate and work on the project.

Remember that the specific folder structure and organization may vary depending on the size and complexity of your project. The above structure serves as a general guideline and can be adapted to suit your specific needs.

