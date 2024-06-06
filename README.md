# Miele API Proof of Concept for ESP32

This project demonstrates a proof of concept for integrating Miele's API with an ESP32 embedded system. The provided code handles the process of authenticating with the Miele API, obtaining an OAuth token, and setting up the ESP32 to interact with Miele's smart home appliances. 

## Table of Contents

- [Miele API Proof of Concept for ESP32](#miele-api-proof-of-concept-for-esp32)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Prerequisites](#prerequisites)
  - [Hardware Requirements](#hardware-requirements)
  - [Software Requirements](#software-requirements)
  - [Getting Started](#getting-started)
    - [Setting Up Your ESP32](#setting-up-your-esp32)
    - [Configuring Miele API](#configuring-miele-api)
    - [Running the Code](#running-the-code)
  - [Miele API Authentication Flow](#miele-api-authentication-flow)
  - [Code Explanation](#code-explanation)
    - [loginMieleAPI Function](#loginmieleapi-function)
  - [Useful Links](#useful-links)
  - [License](#license)

## Introduction

Miele is a well-known manufacturer of high-end domestic appliances and commercial equipment. They provide a third-party API that allows developers to interact with their smart appliances programmatically. This project provides a foundational approach to authenticating and interacting with Miele's API using an ESP32 board, which is a popular choice for IoT applications due to its powerful microcontroller and Wi-Fi capabilities.

## Prerequisites

Before starting, ensure you have the following:

- A basic understanding of C++ and embedded systems.
- Familiarity with ESP32 and its development environment.
- A registered developer account with Miele to access their API credentials.

## Hardware Requirements

- ESP32 development board (e.g., ESP32-WROOM-32).
- USB cable to connect the ESP32 to your computer.
- A computer with the Arduino IDE installed.

## Software Requirements

- **Arduino IDE**: The integrated development environment for coding and uploading firmware to your ESP32.
- **ESP32 Board Support**: Installed via the Arduino Board Manager.
- **HTTPClient Library**: For handling HTTP requests.

## Getting Started

### Setting Up Your ESP32

1. **Install the Arduino IDE**: Download and install from [Arduino's official website](https://www.arduino.cc/en/software).
2. **Add ESP32 Board Support**:
   - Open Arduino IDE.
   - Go to `File` -> `Preferences`.
   - Add `https://dl.espressif.com/dl/package_esp32_index.json` to the `Additional Boards Manager URLs`.
   - Go to `Tools` -> `Board` -> `Boards Manager` and search for "ESP32".
   - Install the package named "esp32" by Espressif Systems.
3. **Select the ESP32 Board**:
   - Go to `Tools` -> `Board`, and choose your specific ESP32 model (e.g., `ESP32 Dev Module`).

### Configuring Miele API

1. **Register as a Developer**: Sign up on the [Miele developer portal](https://developer.miele.com/) and create a new application to obtain `client_id` and `client_secret`.
2. **Set Up API Credentials**: You will need to configure the following in your code:
   - `client_id`: Your application's client ID.
   - `client_secret`: Your application's client secret.
   - `redirect_uri`: The URI that Miele will redirect to after authentication.
   - `scope`: Permissions needed (e.g., `read`, `write`).

### Running the Code

1. **Clone the Repository**: Download the project code from [GitHub repository](https://github.com/your-username/miele-esp32-poc).
2. **Open the Project**: Open the `.ino` file in the Arduino IDE.
3. **Configure Wi-Fi Credentials**: Update the Wi-Fi SSID and password in the code.
4. **Upload the Code**: Connect your ESP32 to your computer and upload the code via the Arduino IDE.

## Miele API Authentication Flow

1. **Initiate Login**: The ESP32 sends a GET request to obtain the authorization URL.
2. **OAuth Authentication**: ESP32 posts user credentials to the authorization endpoint.
3. **Retrieve Authorization Code**: Extract the authorization code from the redirected URL.
4. **Exchange for Token**: Send a POST request to exchange the authorization code for an access token.
5. **Access API**: Use the obtained token to interact with Miele appliances.

## Code Explanation

### loginMieleAPI Function

This function handles the entire OAuth 2.0 authentication flow:

1. **Initialize HTTP Client**: Set up the HTTP client for requests.
2. **Request Authorization URL**: Construct and send a GET request to obtain the authorization URL.
   ```cpp
   String Request = authorizationBaseURL + "?response_type=code&client_id=" + clientId + "&redirect_uri=" 
                    + testurl + "&scope=" + scope_read + "%20" + scope_write + "&state=" + state;
    ```
3. **Send User Credentials**: Post the user's email and password to Miele's OAuth endpoint.
```cpp

String postData = "email=" + username_api + "&password=" + password_api + "&response_type=code&redirect_uri=" + redirectUri + "&state=" + state + "&client_id=" + clientId + "&vgInformationSelector=de-AT";
```
4. **Extract Authorization Code**: Parse the redirection URL to get the code parameter.
5. **Exchange for Access Token**: Post the authorization code to obtain the access token.
```cpp
String postData_token = "grant_type=authorization_code&code=" + code + "&redirect_uri=" + redirectUri + "&client_id=" + clientId + "&client_secret=" + clientSecret;
```
6. **Handle JSON Response**: Parse the JSON response to extract the access token and other details.

## Useful Links

Here are some resources that will help you understand the tools and technologies used in this project:

- [Miele Developer Portal](https://developer.miele.com/): The official portal for Miele's API, where you can register and obtain your API credentials.
- [ESP32 Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/index.html): Comprehensive documentation for the ESP32, including setup guides, API references, and more.
- [Arduino IDE Download](https://www.arduino.cc/en/software): Download the Arduino Integrated Development Environment (IDE) for coding and uploading firmware to your ESP32.
- [OAuth 2.0 Overview](https://oauth.net/2/): A detailed overview of the OAuth 2.0 protocol, which is used for the authentication process in this project.

## License

This project is licensed under the GNU License. See the [LICENSE](LICENSE) file for details.

---

Feel free to modify the code to suit your specific needs and explore the full potential of integrating Miele's smart appliances with the ESP32 platform.

For any issues or contributions, please refer to the [GitHub Issues](https://github.com/Wegiwonka/miele-home-integration-ESP32/issues) section of the repository.

Happy coding!

