One of the coolest things about .Net Core is it's ability to run on systems other than Windows such as Linux and macOS. Unfortunately, that means that if you're targeting multiple platforms, and using continuous integration, you're going to have set up additional build processes for your non-windows target platforms.

I've been recently building a cross-platform version of Barista, called BaristaCore, using .Net Core and wanted xplat CI in my workflow. Having separate VMs (macOS, win and ubuntu) and manually testing is a workable solution in my scenario, but it's a chore to switch VMs and doing these things manually is orthogonal to modern software development. So I went looking for a modern solution.

In the past, I've been using Visual Studio Online and Visual Studio Build to perform CI. Recently, VSO has provided non-windows build capabilities (XCode, others via Gradle) but I wanted to see what's out there in terms of "free for open source projects".


After surveying the marketplace, I settled on Appveyor and TravisCI. Appveyor provides Windows based builds and TravisCI provides builds on Ubuntu 14.04 and XCode. I've also used CircleCI in the past which also provides XCode based builds, but this is a paid-for service.

The first step is to log into each CI host and granting them access to the Github-based repository that you want CI for. That's fortunately a very straight forward task and you can easily see in GitHub all the integrations that you have connected on a per-repository basis:

​![alt text](/img/2016-11-24-cross-platform-continuous-integration-with-net-core-02.png "Github TravisCI Services")
​![alt text](/img/2016-11-24-cross-platform-continuous-integration-with-net-core-01.png "Github Appveyor WebHooks")


Once the integration is set up with a CI host, the workflow on github commit is the following:

1) When your source code is committed into Github, Github sends a POST to any registered webhooks/services. In this case, Appveyor and TravisCI.
2) On receiving the POST and associating it with your account, the Appveyor and TravisCI know to go ahead and queue a build. The target environment is specified in a configuration file contained in your repo or manually through the CI host.
3) When it's time to start your build, the host creates a new docker-based container and clones the code associated with your commit from Github into it.
4) Once cloned, the host looks for any configuration steps in a configuration file, such as pre-build tasks. Appveyor allows for PS or standard cmd shell commands to be executed, while TravisCI is all bash.
5) The host runs through any setup scripts (failing if there any errors) then runs the build and test scripts.
6) If the test script returns a 0 code, your build has succeeded. Additionally Appveyor knows (through a custom logger in the environment) the unit tests run and will pull them into a separate page.

Once the integration is all set up, you get neat notices in Github that indicate the success or failure of your tests associated with your commits.

​![alt text](/img/2016-11-24-cross-platform-continuous-integration-with-net-core-03.png "Github Checks")

As an aside, if you want to build your own integrations, it's possible to do so -- Microsoft actually has a set of steps on their projects that they have integrated with Github to perform and ensure additional tasks.

​![alt text](/img/2016-11-24-cross-platform-continuous-integration-with-net-core-04.png "Microsoft Github Checks")

 
And of course, both CI hosts have badges that you can use in your readme.md to indicate the build status:

​![alt text](/img/2016-11-24-cross-platform-continuous-integration-with-net-core-05.png "TravisCI Appveyor Badges")

Both CI Hosts give a detailed history of your builds

​![alt text](/img/2016-11-24-cross-platform-continuous-integration-with-net-core-06.png "TravisCI Build history")

Additionally, Appveyor will show you the success of each unit test in your suite:

​![alt text](/img/2016-11-24-cross-platform-continuous-integration-with-net-core-07.png "Appveyor Unit Test Results")

The fun part in doing all of this is that if you are using leading-edge tools, such as .Net Core 1.1, you'll need to customize the pre-build tasks to suit your needs.

Currently, Appveyor doesn't include .Net Core 1.1 as part of their container baseline, so I needed to create a script to pull down and install .Net Core 1.1 as a pre-build task.
This can be found here:
https://github.com/BaristaLabs/BaristaCore/blob/master/appveyor.yml

This is true also for TravisCI, which (at this point) knows nothing about .Net Core. Additionally, since my solution uses native libraries (ChakraCore) that need to be re-built in the target environment, I needed to customize the build-script to pull ChakraCore dependencies, build ChakraCore and place them in a location where my .Net Core assemblies would pick them up.
https://github.com/BaristaLabs/BaristaCore/blob/master/.travis.yml



After much trial and error, I was able to get the build scripts just right, and use some advanced capabilities such as host-level folder caching to reduce the time it takes to perform the CI build and report status.


Anyone else using Cross-Platform Continuous Integration with .Net Core? Would love to hear any experiences.

Thanks,