---
title: "Add Authentication to Streamlit Apps"
date: 2025-03-12T19:54:53-07:00

categories:
  - Technology
tags:
  - Streamlit
  - User Authentication
  - streamlit_authenticator
  - AWS Amplify
---

# Secure Your Streamlit Apps: Implementing User Authentication

Streamlit empowers you to build powerful data applications quickly. But what if you need to restrict access to sensitive data or features? User authentication is the answer. This post will guide you through two popular methods for adding user authentication to your Streamlit applications: `streamlit_authenticator` and AWS Amplify.

## Why User Authentication Matters in Streamlit

User authentication adds a crucial layer of security, ensuring that only authorized users can access your application. This is essential for:

* **Protecting sensitive data:** Prevent unauthorized access to confidential information.
* **Controlling access to features:** Restrict certain functionalities to specific user roles.
* **Personalizing user experiences:** Tailor the application based on individual user preferences.
* **Complying with regulations:** Meet data privacy and security requirements.

## Method 1: Streamlit Authenticator - Simple and Local

`streamlit_authenticator` is a community-driven library that provides a straightforward way to implement user authentication directly within your Streamlit application. It's ideal for local deployments or simple applications where you don't need complex identity management.

### Installation

```bash
pip install streamlit-authenticator
```

### Implementation

2.  **Generate Hashes:** 

* `streamlit_authenticator` uses hashed passwords for security. You'll need to generate these hashes using the library's utility function.

    ```python
    import streamlit_authenticator as stauth
    import yaml
    from yaml.loader import SafeLoader

    with open('config.yaml') as file:
        config = yaml.load(file, Loader=SafeLoader)

    authenticator = stauth.Authenticate(
        config['credentials'],
        config['cookie']['name'],
        config['cookie']['key'],
        config['cookie']['expiry_days'],
        config['preauthorized']
    )

    names = ['John Smith', 'Rebecca Briggs']
    usernames = ['jsmith', 'rbriggs']
    passwords = ['abc', '123']

    hashed_passwords = stauth.Hasher(passwords).generate()

    config['credentials']['usernames'][usernames[0]]['name'] = names[0]
    config['credentials']['usernames'][usernames[0]]['password'] = hashed_passwords[0]
    config['credentials']['usernames'][usernames[1]]['name'] = names[1]
    config['credentials']['usernames'][usernames[1]]['password'] = hashed_passwords[1]

    with open('config.yaml', 'w') as file:
        yaml.dump(config, file, default_flow_style=False)
    ```

    * This code generates hashes for the provided passwords and updates the `config.yaml` file.
    * **config.yaml:** This file stores user credentials, cookie settings, and preauthorized users.
    * Example of a config.yaml file.
        ```yaml
        credentials:
          usernames:
            jsmith:
              name: John Smith
              password: $2b$12$xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
            rbriggs:
              name: Rebecca Briggs
              password: $2b$12$yyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
        cookie:
          name: some_name
          key: some_key
          expiry_days: 30
        preauthorized:
          emails:
            - [email address removed]
        ```
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- cpa -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-2843564932689995"
     data-ad-slot="3526097725"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

2.  **Integrate Authentication:** 

    ```python
    import streamlit as st
    import streamlit_authenticator as stauth
    import yaml
    from yaml.loader import SafeLoader

    with open('config.yaml') as file:
        config = yaml.load(file, Loader=SafeLoader)

    authenticator = stauth.Authenticate(
        config['credentials'],
        config['cookie']['name'],
        config['cookie']['key'],
        config['cookie']['expiry_days'],
        config['preauthorized']
    )

    name, authentication_status, username = authenticator.login('Login', 'main')

    if authentication_status:
        authenticator.logout('Logout', 'main')
        st.write(f'Welcome *{name}*')
        st.title('Some content')
    elif authentication_status is False:
        st.error('Username/password is incorrect')
    elif authentication_status is None:
        st.warning('Please enter your username and password')
    ```

    * The `authenticator.login()` function displays the login form.
    * The `authentication_status` variable indicates whether the login was successful.
    * You can use `authenticator.logout()` to provide a logout button.

### Advantages

* Simple and easy to implement.
* Works locally without external dependencies.
* Suitable for small to medium-sized applications.

### Disadvantages

* Limited scalability and features compared to cloud-based solutions.
* Requires manual management of user credentials.
* Less secure than cloud based solutions if the config.yaml file is not properly protected.

## Method 2: AWS Amplify - Scalable and Secure

AWS Amplify provides a comprehensive platform for building secure and scalable web and mobile applications. It integrates seamlessly with AWS Cognito for user authentication.

### Prerequisites

* An AWS account.
* AWS CLI installed and configured.
* Node.js and npm installed.

### Implementation

1.  **Initialize Amplify Project:**

    ```bash
    amplify init
    ```

    * Follow the prompts to configure your project.
2.  **Add Authentication:**

    ```bash
    amplify add auth
    ```

    * Choose the default configuration or customize your settings.
3.  **Deploy Authentication:**

    ```bash
    amplify push
    ```

    * This creates an AWS Cognito user pool for your application.
4.  **Integrate with Streamlit:**

    * You'll need to use the AWS SDK for Python (Boto3) to interact with Cognito.
    * Install Boto3: `pip install boto3`
    * Use the generated user pool id, and client id from amplify to authenticate users.
    * Example code:

    ```python
    import streamlit as st
    import boto3

    cognito_client = boto3.client('cognito-idp', region_name='your_region')

    def authenticate_user(username, password, client_id, user_pool_id):
        try:
            response = cognito_client.initiate_auth(
                AuthFlow='USER_PASSWORD_AUTH',
                AuthParameters={
                    'USERNAME': username,
                    'PASSWORD': password,
                },
                ClientId=client_id,
                UserPoolId=user_pool_id
            )
            return True, response['AuthenticationResult']['IdToken']
        except cognito_client.exceptions.NotAuthorizedException:
            return False, None
        except Exception as e:
            st.error(f"An unexpected error occured: {e}")
            return False, None

    username = st.text_input("Username")
    password = st.text_input("Password", type="password")

    if st.button("Login"):
        success, token = authenticate_user(username, password, "your_client_id", "your_user_pool_id")
        if success:
            st.success("Login successful!")
            st.session_state.token = token
            #Now you have the token, and can use it in your application.
        else:
            st.error("Login failed.")

    if 'token' in st.session_state:
        st.write("Logged in content")

    ```

### Advantages

* Highly scalable and secure.
* Integrates with AWS Cognito for robust identity management.
* Offers advanced features like multi-factor authentication and social login.
* Provides a full suite of backend services.

### Disadvantages

* More complex to set up compared to `streamlit_authenticator`.
* Requires an AWS account and familiarity with AWS services.
* Can incur costs based on usage.

## Summary

Adding user authentication to your Streamlit applications is crucial for security and access control. `streamlit_authenticator` is a simple and effective solution for local deployments, while AWS Amplify provides a scalable and robust platform for production applications. Choose the method that best suits your needs and application requirements.