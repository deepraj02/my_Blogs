---
title: "Streamlining Your Flutter Package Publishing Workflow: Automating with GitHub Actions🤯"
seoTitle: "How to automate Flutter Package Publishing"
seoDescription: "How to automate Flutter Package Publishing
How to publish flutter package with github actions"
datePublished: Thu Apr 13 2023 11:46:26 GMT+0000 (Coordinated Universal Time)
cuid: clgf21jbl000c09l9e26n0sfu
slug: streamlining-your-flutter-package-publishing-workflow-automating-with-github-actions
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/SyvsTmuuZyM/upload/297361cb2a199ababf3e3d88dfae07c5.jpeg
tags: github, dart, flutter, package, github-actions-1

---

Have you ever experienced time-consuming difficulty manually publishing your Flutter packages? Do you wish there were a method to speed things up and eliminate human error? You're in luck if so! You may automate your package publishing procedure and spare yourself time, hassle, and frustration by using the strength of GitHub Actions.

This Blog will guide you through the process of setting up a GitHub Workflow to automatically publish your Flutter package to [pub.dev](http://Pub.dev). This tutorial will give you the information and resources you need to advance your package publishing process, whether you're an experienced developer or just learning Flutter.

Let's Get Started✌️:

%[https://media.giphy.com/media/3oKIPu1AxMWB2xlwl2/giphy.gif] 

# Prerequisites:

* [Pub.dev](https://pub.dev) Account.
    
* Github Repository for the Package.
    
* A Flutter Package to Publish.
    

## A Little Back-Story:

A few weeks back I started working on my own Flutter Package [neubrutalism\_ui](https://pub.dev/packages/neubrutalism_ui), which is a UI kit packed with amazing Neubrutalist styled widgets, and I was having difficulty deploying the package again and again with changes, I was trapped in an infinite loop where I was doing the same thing manually every time I had to update the package with changes.

I started looking for solutions to automate the process, after hours of trying different solutions from StackOverflow and this [Blog](https://birjuvachhani.medium.com/publish-your-flutter-dart-package-using-github-actions-99144cbf15ae) but none of them were working, then I found [https://github.com/k-paxian/dart-package-publisher](https://github.com/k-paxian/dart-package-publisher) this repo, which worked perfectly.

%[https://media.giphy.com/media/EDt1m8p5hqXG8/giphy.gif] 

## Step 1:

* Navigate to your package on pub.dev (manually deploy your package once - I found this way helpful)
    
* Click on the Admin Section
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681384073684/150e0e91-d0e6-448c-9751-a8ff1f13cfaf.png align="center")
    
    Scroll down to the 'Automated Publishing' Section and enable the 'Enable publishing from GitHub Actions' button then add the repository and the version pattern.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681384165065/7d220c0e-bd40-47ad-bdb5-6e60ff1fb3b2.png align="center")
    

## Step 2:

Copy your pub-credentials.json:

* **For Windows**
    
    ```bash
    %APPDATA%/dart/pub-credentials.json
    ```
    
* **For Linux**
    
    ```bash
    $HOME/.config/dart/pub-credentials.json
    ```
    
* **For MacOs**
    
    ```bash
    ~/Library/Application Support/dart/pub-credentials.json
    ```
    

* **NOTE:** *Note: if you don’t have or want to create a new pub-credentials.json. then run the following command in your project path and you will get the file in the specified path*
    
    ```bash
    dart pub publish --dry-run
    dart pub publish
    ```
    

## Step 3:

Create a new Github Secret and store your pub-credential data there.

%[https://scribehow.com/shared/How_to_Add_Repository_Secrets_and_Variables_on_GitHub__Kj5iSTMfSB-nwFunRXgaRA] 

## Step 4:

* Create a `.github/workflows` directory in your project directory.
    
* Create a new file `publish.yaml` in the above-created directory.
    

## Step 5:

Writing the workflow to automate the deployment🔥🔥🔥.

```yaml
name: Publish Flutter Package

on:
  push:
    branches:
      - master

jobs:
  publish-package:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Flutter
        uses: subosito/flutter-action@v2

      - name: Get dependencies
        run: flutter pub get

      - name: Analyze code
        run: flutter analyze

      - name: Format code
        run: dart format --fix .

      - name: Check publish warnings
        run: dart pub publish --dry-run

      - name: Publish package
        uses: k-paxian/dart-package-publisher@v1.5.1
        with:
          credentialJson: ${{ secrets.CREDENTIAL_SECRET }} 
          flutter: true
          skipTests: true
```

### Explanation:

1. The above action will be triggered when we push changes to the `master` branch. (I recommend creating 2 branches one for development and testing and the other for deployment, think of them as `prod` and `dev` environment)
    
2. Creates a job for a new build that will run on ubuntu-latest
    
3. Set up configuration, install dependencies, analyze etc
    
4. The `k-paxian/dart-package-publisher@v1.5.1` is the plugin that will take care of our package deployment in [pub.dev](http://pub.dev).
    
5. Provide all the parameters like `credentialJson`which we created in GitHub Secret and so on.
    

**\[Note\]:** *To trigger a new build, commit all your changes and head over to your root repository and there you will see create a new release build.*

# And It's Done ✅

%[https://media.giphy.com/media/3o7qDEq2bMbcbPRQ2c/giphy.gif] 

If you found this article interesting, you may want to check out a few other things I wrote about *Flutter.*

* [**Flutter with Neubrutalism: A Guide🔥**](https://blogs.deeprajbaidya.co/add-neubrutalism-design-to-your-flutter-app)
    
* [**Best VS-Code Extension for Developers**](https://blogs.deeprajbaidya.co/top-10-vs-code-extentions-sept-2022)
    

Please don’t hesitate to write to me to [**deepraj@oncampus.tech**](mailto:deepraj@oncampus.tech), I’m always happy to hear from people reading my articles.

Connect with me: [**Linkedin**](https://www.linkedin.com/in/deeprajbaidya/)**,** [**Github**](https://github.com/deepraj02)**,** [**Twitter**](https://twitter.com/Deepraj022)