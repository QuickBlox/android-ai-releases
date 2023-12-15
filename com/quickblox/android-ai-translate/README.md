<div align="center">

<p>
		<a href="https://discord.gg/c6bxq9BC"><img src="https://img.shields.io/discord/1042743094833065985?color=5865F2&logo=discord&logoColor=white&label=QuickBlox%20Discord%20server&style=for-the-badge" alt="Discord server" /></a>
</p>

</div>

# Overview

AITranslate functionality that helps users simplify cross-lingual communication by providing instant message translation services. The library is based on OpenAI functional and provides the simplest way to communicate with API.

# Installation

Implementing this feature is easy. To utilize the QuickBlox AITranslate library, you only need to follow a few simple steps.

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
    
    // below we have a link to QuickBlox AITranslate library
    implementation "com.quickblox:android-ai-translate:2.0.0"
}
```

Version 2.0.0 can be updated to the latest version. Discover the latest release and review the entire release history in our [repository](https://github.com/QuickBlox/android-ai-releases/releases).

## Usage
The QuickBlox AITranslate library can be used with two different approaches. Either directly, using the library with raw Open AI token, or with a QuickBlox session token via a proxy.

Direct approach – While this method may be suitable during the development phase, (because it allows the quick testing of functionality without having to spend time rolling out the proxy server), it’s not recommended when the app is released. This is because care should always be taken to hide tokens, for example, by adding the token to the local.properties file. But this direct approach involves using an OpenAI token directly by allowing our AI library to directly communicate with the OpenAI API.

Proxy approach – This is the preferred method. It is far more secure because the OpenAI token is encapsulated inside the proxy server. Using a QuickBlox session token, the AI library will communicate via a proxy server (not directly) with OpenAI API.

### Direct Approach
To invoke the QuickBlox AITranslate library method you need to add necessary imports and invoke the executeByOpenAIToken method like in the code snippet below.

```
// below we have required imports for the AITranslate library
import com.quickblox.android_ai_translate.Languages
import com.quickblox.android_ai_translate.QBAITranslate
import com.quickblox.android_ai_translate.callback.Callback
import com.quickblox.android_ai_translate.exception.QBAITranslateException
import com.quickblox.android_ai_translate.message.Message
import com.quickblox.android_ai_translate.settings.TranslateSettingsImpl

 private fun translateWithOpenAITokenAsync() {
        val text = "The text to translate"

        val openAIToken = "open_ai_token" // our raw open ai token
        val language = Languages.SPANISH
        val translateSettings = TranslateSettingsImpl(openAIToken, language)

        val messages = mutableListOf<Message>() // messages history (optional parameter)

        QBAITranslate.translateAsync(text, translateSettings, object : Callback {
            override fun onComplete(translation: String) {
                // here will be a result
            }

            override fun onError(exception: QBAITranslateException) {
                // oops, something goes wrong
            }
        }, messages)
    }

    private fun translateWithOpenAITokenSync() {
        val text = "The text to translate"

        val openAIToken = "open_ai_token" // our raw open ai token
        val language = Languages.SPANISH
        val translateSettings = TranslateSettingsImpl(openAIToken, language)

        val messages = mutableListOf<Message>() // messages history (optional parameter)

        try {
            val translation = QBAITranslate.translateSync(text, translateSettings, messages)
        } catch (exception: QBAITranslateException) {
            // need to handle exception if we need
        }
    }
```

### Proxy Approach

```
// below we have required imports for QuickBlox AITranslate library
import com.quickblox.android_ai_translate.Languages
import com.quickblox.android_ai_translate.QBAITranslate
import com.quickblox.android_ai_translate.callback.Callback
import com.quickblox.android_ai_translate.exception.QBAITranslateException
import com.quickblox.android_ai_translate.message.Message
import com.quickblox.android_ai_translate.settings.TranslateSettingsImpl

private fun translateWitProxyServerAsync() {
    val text = "The text to translate"

    val qbToken = "quickblox_token" // our QuickBlox session token
    val serverProxyUrl = "https://my.proxy-server.com" // our proxy server url
    
    val language = Languages.SPANISH
    val translateSettings = TranslateSettingsImpl(qbToken, serverProxyUrl, language)

    val messages = mutableListOf<Message>() // messages history (optional parameter)

    QBAITranslate.translateAsync(text, translateSettings, object : Callback {
        override fun onComplete(translation: String) {
            // here will be a result
        }
        override fun onError(exception: QBAITranslateException) {
            // oops, something goes wrong
        }
    }, messages)
}

private fun translateWitProxyServerSync() {
    val text = "The text to translate"

    val qbToken = "quickblox_token" // our QuickBlox session token
    val serverProxyUrl = "https://my.proxy-server.com" // our proxy server url
    
    val language = Languages.SPANISH
    val translateSettings = TranslateSettingsImpl(qbToken, serverProxyUrl, language)

    val messages = mutableListOf<Message>() // messages history (optional parameter)

    try {
        val answer = QBAITranslate.translateSync(text, translateSettings, messages)
    } catch (exception: QBAITranslateException) {
        // need to handle exception if we need
    }
}
```

We recommended using the proxy server method. Please ensure you use the latest method which you can in our [repository](https://github.com/QuickBlox/qb-ai-assistant-proxy-server/releases).

##
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

AITranslate is released under the [MIT License](LICENSE.md).

## Contribution

We welcome contributions to improve AITranslate. If you find any issues or have suggestions, feel free to open an issue or submit a pull request on GitHub.
Join our Discord Server: https://discord.gg/GrynPeHMaa

Happy coding! 🚀
