> If you have worked on a reasonably large project somewhere, chances are that the thought crossed your mind sometimes about
making the code you produced available for others to use. In this article I try to sum up the Pros and the Cons about
doing so and provide a few pointers why it is useful if you have never worked on OSS before.

Working on open source software (OSS in short) on your own time is fun. You get to choose what to work on, when to do it and how to do it. If you produce something which is useful for others, people will eventually start to use it and this is where interesting things start to happen.

## Pull requests

Most of the time your users won't have the time to contribute something useful but there are some programmers who like to submit a patch or two which will provide insight to you about how your program is used. Given that you get a pull request which has reasonably good quality (fix or new feature with a bunch of unit test) you will be able to deduce a bunch of things:
- You will see __exactly__ how your program is being used
- You will know __how__ others would like to use your program
- You will gain insight to the perspective to the developer who submitted the PR. This can be incredibly useful

## Bug reports

There will be some users who won't (or can't ) contribute to your project but want to leave a comment about a problem they encountered. Most platforms which host OSS projects support this out of the box ([GitHub](https://github.com/) for example). Although they are not as useful compared to a PR you can consider them as functional tests performed for free. Like with PR-s bug reports also provide you with insight about how your program is being used.

## Feature requests

From time to time you'll get some new ideas from the outside world in the form of feature requests. These are invaluable because they are solutions for a *real business need*. They also provide you with information about which direction you may steer the development especially if you are puzzled where you should go from where you stand with the project.

## Contributors and committers

It is possible that someone is so interested in your project that they become contributors and if it turns out that you can work together well they can also become committers. Needless to say that this is the best thing that can happen and it is also a sign of your project being in good shape.

## Possible drawbacks

Although there are a lot of advantages of OSS there are also some drawbacks which you will face sooner or later. Most of these can be mitigated but preparing for them is important if you don't want your project to become a time sink.

### Low quality contributions

It is just a matter if time when you will face the first PR from hell. It will either brake your build, lack tests or worse. I've seen PRs which refactor major parts of a program just to add a generic type with little or no benefit and code which simply brake the build. There are some things however which you can do __before__ you are presented with something like this:

- Include a [Code of Conduct](https://en.wikipedia.org/wiki/Code_of_conduct) in your project. A good example can be found [here](http://contributor-covenant.org/version/1/1/0/). This is important because you don't want to seem ungrateful when someone spends his/her time on your project and you need to refuse a PR. By pointing them to the COC you can prevent losing face.
- Add a [Contributor License Agreement](https://en.wikipedia.org/wiki/Contributor_License_Agreement). It is important to clarify who owns the [Intellectual Property](https://en.wikipedia.org/wiki/Intellectual_property) of the project. Failing to do so can lead to nasty disputes which won't make you happy. A nice example can be found [here](https://github.com/ReactiveX/RxJava/blob/2.x/CONTRIBUTING.md)
- Use [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration). If you do it right you can delegate almost all quality assurance to a computer and you can also have them tell your contributors what they did wrong. Platforms like GitHub can be integrated with [Travis](https://travis-ci.org/) for example and also code coverage tools like [CodeCov](https://codecov.io/) which can even comment on pull requests like in [this](https://github.com/Hexworks/hexameter/pull/24) one.

### Too much buzz around your project

You may start with something small but any project can become the second Death Star if you put in the time and the effort. Also if your project gains attention you can find yourself in the situation when you are flooded with feature requests and bug reports. While these are good for your projects you may find yourself having another full-time job which can consume a lot of your spare time.

This can be worked around by letting proven contributors become committers and using a proper Kanban/Scrum board with a backlog and prioritizing tasks.

### Licensing problems

It is possible that you use a license like MIT which might be too permissive for your use case or AGPL which is too restricting. Choosing the right license is very important so I'd recommend perusing [this](https://choosealicense.com/) website for more information.

## Things to tell your boss

So now you might think that you are ready to open source parts of your project at your workplace or your own hobby project. In the former case you have a sometimes very hard task ahead of you: *convincing your boss*. Here are a few tips you can leverage to make your idea pass:

> - Open sourcing *comes at nearly no cost*
> - You don't have to open source everything just the parts which might be useful for others. This also helps with the modularization of your program.
> - You will get *free* bug reports, contributors or even core committers.
> - Programmers like to work for companies where they know that their code can get open sourced which makes their contributions more visible to the outside world.
> - Not only you but your company also gets some *positive karma*.
> - By making code visible for everyone you will think twice before adding an ugly hack since your professional repuration is on the line. This makes the code have slightly better quality.

I think that the benefits outweight the drawbacks so there is no reason not to try OSS out. Go for it and feel free to share your experiences in the comment section or if you think otherwise.





