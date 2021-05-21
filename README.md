<!--- STARTEXCLUDE --->
# Astra Tik-Tok
*50 minutes, Advanced, [Start Building](#running-astra-tik-tok)*

A simple Tik-Tok clone running on AstraDB that leverages the Document API.
<!--- ENDEXCLUDE --->

![image](./screenshot.jpg)

## Objectives
* Work through a video tutorial to build a simple Tik-Tok clone
* Leverage Netlify and DataStax AstraDB
  
## How this works
We're using Create-React-App and the AstraDB Document API to create a simple Tik-Tok clone.  Follow along in this video tutorial: [https://youtu.be/IATOicvih5A](https://youtu.be/IATOicvih5A).

<!--- STARTEXCLUDE --->
## Running Astra Tik-Tok
Follow the instructions below to get started.

### Video Content:
- [https://youtu.be/IATOicvih5A](https://youtu.be/IATOicvih5A)
- (00:00) Introduction
- (03:05) Creating our Database on DataStax
- (06:52) Setting up our App
- (12:37) Routing Pages
- (18:02) Creating Components
- (28:32) Introduction to Data with Netlify and Stargate
- (30:10) Introduction to using the astrajs/collections
- (34:01) Posting data to our Database (creating dummy Tik Tok posts)
- (34:01) Adding authorization to access our Database
- (43:10) Getting data from our Database (getting all our Tik Tok posts)
- (50: 32) Viewing all our Data
- (51:56) Rendering components based on our Data
- (01:17:01) Editing our Data (following/unfollowing a user)
- (01:32:57) Adding new Data to our Database (creating a Tik Tok post)

### If you did like this video, please hit the Like and Subscribe button so I know to make more!
- Twitter: https://twitter.com/ania_kubow
- YouTube: https://youtube.com/aniakubow
- Instagram: https://instagram.com/aniakubow

## Prerequisites
Let's do some initial setup by creating a serverless(!) database.
* git installed on your local system
* github account
* [node 15 and npm 7 or later](https://www.whitesourcesoftware.com/free-developer-tools/blog/update-node-js/)

### 1. Login/Register to Astra and create database

Click the button to login or register with Datastax

<a href="https://astra.datastax.com/register?utm_source=github&utm_medium=referral&utm_campaign=todo-astra-jamstack-netlify"><img src="https://dabuttonfactory.com/button.png?t=Create+Astra+Database&f=Calibri-bold&ts=20&tc=fff&hp=40&vp=10&c=8&bgt=unicolored&bgc=6fa8dc" /></a>
- <details><summary>Show me!</summary>
    <img src="https://github.com/datastaxdevs/workshop-spring-stargate/raw/main/images/tutorials/astra-create-db.gif?raw=true" />
</details>

**Use the following values when creating the database**
|Field| Value|
|---|---|
|**database name**| `tiktok_workshop_db` |
|**keypace**| `tktkposts` |
|**Cloud Provider**| *Use the one you like, click a cloud provider logo,  pick an Area in the list and finally pick a region.* |

### 2. Deploy to Netlify
- <details><summary> What does the netlify deploy button do?</summary>The Netlify deploy button will:<ul>
    <li>Create a new repository for you on Github</li>
    <li>Create a site on Netlify</li>
    <li>Link the two together.</li></ul>
</details>

- Click the button to deploy

  [![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/synedra/netlify-astra-example)
 * <details><summary>Show me!</summary>
    <img src="https://github.com/datastaxdevs/workshop-spring-stargate/raw/main/images/tutorials/astra-create-token.gif?raw=true" />
    </details>

This will take a few minutes.

  * Click on `Site deploy in progress`, 
    <details>
    <summary>Show me! </summary>
    <img src="/images/deploy-1.png" />
    </details>

  * Click the top deploy link to see the build process.
    <details>
    <summary>Show me! </summary>
    <img src="/images/deploy-2.png" />
    </details>

  * Wait until the build complete `Netlify Build Complete`,  **When you see Pushing to repository** you're ready to move on.
    <details>
    <summary>Show me! </summary>
    <img src="/images/deploy-3.png" />
    </details>

  * Scroll up to the top and click on the site name (it'll be after {yourlogin}'s Team next to the Netlify button).
    <details>
    <summary>Show me! </summary>
    <img src="/images/deploy-4.png" />
    </details>

### 3. Clone your GitHub repository

  * Click on the `GitHub` in `Deploys from GitHub` to get back to your new repository.  Scroll to where you were in the README.
    <details>
    <summary>Show me! </summary>
    <img src="/images/deploy-5.png" />
    </details>

### 4. Launch GitPod IDE
- Click the button to launch the GitPod IDE from **YOUR** repository

  [![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#referrer)
 * <details><summary>Show me!</summary>
    <img src="https://github.com/datastaxdevs/workshop-spring-stargate/raw/main/images/tutorials/astra-create-token.gif?raw=true" />
    </details>

### 5. Install the Netlify CLI (Command Line Interface)
 * In the `whatever directory` run the following command to install the netlify-cli
 ```
 npm install -g netlify-cli
```

### 6. Generate application token to securely connect to the database

Following the [Documentation](https://docs.datastax.com/en/astra/docs/manage-application-tokens.html) create a token with `Database Admnistrator` roles.

- Go the `Organization Settings`

- Go to `Token Management`

- Pick the role `Database Admnistrator` on the select box

- Click Generate token

 * <details><summary>Show me!</summary>
    <img src="https://github.com/datastaxdevs/workshop-astra-tik-tok/raw/main/tutorial/image/astra-create-token.gif?raw=true" />
    </details>

This is what the token page looks like. 
 * Click the **`Download CSV`** button. You are going to need these values here in a moment.

![image](tutorial/images/astra-token.png?raw=true)

Notice the clipboard icon at the end of each value.

- `clientId:` We will use it as a username to contact Cassandra

- `clientSecret:` We will use it as a password to contact Cassandra

- `appToken:` We will use it as a api Key to interact with APIS.

To know more about roles of each token you can have a look to [this video.](https://www.youtube.com/watch?v=nRqu44W-bMU)

### 7. Configure and connect database
 * In the repository directory run the following command to set up your Astra environment. This will verify the database you created earlier or create a new one for you if it can't find your database.
 ```
 npm exec astra-setup tiktok_workshop_db tktkposts
```

<details>
<summary>What does astra-setup do?</summary>
    To setup your ASTRA instance, you want to run `npm exec astra-setup`

    This will do the following:
    * Have you go to your [Astra Database](https://datastx.io/workshops) to register or login. There is no credit card required to sign up. The 'Pay as you go' option gives you a huge amount of transactions for free:
        * 30 million reads
        * 5 million writes
        * 40 gigabytes of storage
    * Give steps to grab a Database Administrator Token and paste it into the input field
    * Ask you what database you want to use (default, existing, create)
    * Create or access the database
    * Create/update an .env file in the project root
    * Create/update an .astrarc file in your home directory
        * This can be used by httpie-astra `pip3 install httpie-astra`
        * It can also be used by the @astra/collections and @astra/rest node modules

    ## Specify the database and keyspace
    You can run the script and tell it which database/keyspace to use by using:
    `npm exec astra-setup databasename keyspacename`
</details>

### 8. Connect Netlify to your site
  * `netlify login` - this will pop up a browser to authenticate with netlify.  
  * `netlify link` - this will link your workspace to the associated site
  * `netlify env:import .env` - this will take the .env file created by astra-setup and upload it to netlify.
  * `netlify sites:list` - will be used to allow you to execute `npm exec netlify-open


### 9. Launch your app
  * Run the application `netlify dev` and open http://localhost:8080 to view your application:

### 10. Deploy to production
  * Run `npm exec netlify-open`.
  
  You've deployed your app to Netlify!
  ![Netlify Setup Example](./tutorial/images/netlify-livesite.png?raw=true)
  
<!--- ENDEXCLUDE --->