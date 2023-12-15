<div align="center">

<p>
		<a href="https://discord.gg/c6bxq9BC"><img src="https://img.shields.io/discord/1042743094833065985?color=5865F2&logo=discord&logoColor=white&label=QuickBlox%20Discord%20server&style=for-the-badge" alt="Discord server" /></a>
</p>

</div>

# Overview

AI Rephrase functionality that helps users effortlessly modify text with multiple variants, including integration with the OpenAI API.

# Installation

Implementing this feature is easy. To utilize the QuickBlox AI Rephrase library, you only need to follow a few simple steps.

 1. Add the repository
 2. Add dependency

## Step 1. Add the repository
You first need to add the repository to your existing Android application.
Open Gradle (root level) file and add the code as illustrated in the code snippet below. See the dependencyResolutionManagement section in particular:

```
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        
      	// below we have the a link to QuickBlox AI repository
        maven {
            url "https://github.com/QuickBlox/android-ai-releases/raw/main/"
        }
    }
}
```

## Step 2. Add the dependency
Next, you need to add a dependency to the existing Android application.

Open Gradle (application level) file and add the code as illustrated in the snippet below. See the dependencies section.

```
dependencies {
    // some other dependencies
    
    // below we have a link to QuickBlox AI Rephrase library
    implementation "com.quickblox:android-ai-editing-assistant:2.1.0"
}
```

Version 2.0.0 can be updated to the latest version. Discover the latest release and review the entire release history in our [repository](https://github.com/QuickBlox/android-ai-releases/releases).

## Usage
The QuickBlox AI Rephrase library can be used with two different approaches. Either directly, using the library with raw Open AI token, or with a QuickBlox session token via a proxy.

Direct approach – While this method may be suitable during the development phase, (because it allows the quick testing of functionality without having to spend time rolling out the proxy server), it’s not recommended when the app is released. This is because care should always be taken to hide tokens, for example, by adding the token to the local.properties file. But this direct approach involves using an OpenAI token directly by allowing our AI library to directly communicate with the OpenAI API.

Proxy approach – This is the preferred method. It is far more secure because the OpenAI token is encapsulated inside the proxy server. Using a QuickBlox session token, the AI library will communicate via a proxy server (not directly) with OpenAI API.

### Direct Approach
To invoke the QuickBlox AI Rephrase library method you need to add necessary imports and invoke the executeByOpenAIToken method like in the code snippet below.

```
// below we have required imports for QuickBlox AI Rephrase library
import com.quickblox.android_ai_editing_assistant.QBAIRephrase
import com.quickblox.android_ai_editing_assistant.callback.Callback
import com.quickblox.android_ai_editing_assistant.exception.QBAIRephraseException
import com.quickblox.android_ai_editing_assistant.model.ToneImpl
import com.quickblox.android_ai_editing_assistant.settings.SettingsImpl

private fun rephraseWithOpenAIToken() {
    val toneName = "Professional"
    val toneDescription = "Description of tone"
    val tone = ToneImpl(toneName, toneDescription)

    val openAIToken = "open_ai_token"

    val rephraseSettings = SettingsImpl(openAIToken, tone)

    val text = "Hi Bro. Can you help me?"
    QBAIRephrase.rephraseAsync(text, rephraseSettings, object : Callback {
        override fun onComplete(result: String) {
            // here will be result as list of answers
        }
        override fun onError(error: QBAIRephraseException) {
            // oops, something goes wrong
        }
    })
}
```

### Proxy Method

```
// below we have required imports for QuickBlox AI Rephrase library
import com.quickblox.android_ai_editing_assistant.QBAIRephrase
import com.quickblox.android_ai_editing_assistant.callback.Callback
import com.quickblox.android_ai_editing_assistant.exception.QBAIRephraseException
import com.quickblox.android_ai_editing_assistant.model.ToneImpl
import com.quickblox.android_ai_editing_assistant.settings.SettingsImpl

private fun rephraseWitProxyServer() {
    val toneName = "Professional"
    val toneDescription = "Description of tone"
    val tone = ToneImpl(toneName, toneDescription)
    
    val qbToken = "quickblox_token"
    
    val serverProxyUrl = "https://my.proxy-server.com"
    
    val rephraseSettings = SettingsImpl(qbToken, serverProxyUrl, tone)
    
    val text = "Hi Bro. Can you help me?"
    QBAIRephrase.rephraseAsync(text, rephraseSettings, object : Callback {
        override fun onComplete(result: String) {
            // here will be result as list of answers
        }
        override fun onError(error: QBAIRephraseException) {
            // oops, something goes wrong
        }
    })
}
```

We recommended using the proxy server method. Please ensure you use the latest method which you can in our [repository](https://github.com/QuickBlox/qb-ai-assistant-proxy-server/releases).

## OpenAI

To use the OpenAI API and generate answers, you need to obtain an API key from OpenAI. Follow these steps to get your API key:

1. Go to the [OpenAI website](https://openai.com) and sign in to your account or create a new one.
2. Navigate to the [API](https://platform.openai.com/signup) section and sign up for access to the API.
3. Once approved, you will receive your [API key](https://platform.openai.com/account/api-keys), which you can use to make requests to the OpenAI API.

## Proxy Server

Using a proxy server like the [QuickBlox AI Assistant Proxy Server](https://github.com/QuickBlox/qb-ai-assistant-proxy-server) offers significant benefits in terms of security and functionality:

Enhanced Security:
- When making direct requests to the OpenAI API from the client-side, sensitive information like API keys may be exposed. By using a proxy server, the API keys are securely stored on the server-side, reducing the risk of unauthorized access or potential breaches.
- The proxy server can implement access control mechanisms, ensuring that only authenticated and authorized users with valid QuickBlox user tokens can access the OpenAI API. This adds an extra layer of security to the communication.

Protection of API Keys:
- Exposing API keys on the client-side could lead to misuse, abuse, or accidental exposure. A proxy server hides these keys from the client, mitigating the risk of API key exposure.
- Even if an attacker gains access to the client-side code, they cannot directly obtain the API keys, as they are kept confidential on the server.

Rate Limiting and Throttling:
- The proxy server can enforce rate limiting and throttling to control the number of requests made to the OpenAI API. This helps in complying with API usage policies and prevents excessive usage that might lead to temporary or permanent suspension of API access.

Request Logging and Monitoring:
- By using a proxy server, requests to the OpenAI API can be logged and monitored for auditing and debugging purposes. This provides insights into API usage patterns and helps detect any suspicious activities.

Flexibility and Customization:
- The proxy server acts as an intermediary, allowing developers to introduce custom functionalities, such as response caching, request modification, or adding custom headers. These customizations can be implemented without affecting the client-side code.

SSL/TLS Encryption:
- The proxy server can enforce SSL/TLS encryption for data transmission between the client and the server. This ensures that data remains encrypted and secure during communication.

## License

QBAIRephrase is released under the [MIT License](LICENSE.md).

## Contribution

We welcome contributions to improve QBAIRephrase. If you find any issues or have suggestions, feel free to open an issue or submit a pull request on GitHub.
Join our Discord Server: https://discord.gg/GrynPeHMaa

Happy coding! 🚀
