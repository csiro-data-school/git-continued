---
title: "Making git citable"
teaching: 15
exercises: 15
questions:
  - "How can we make a particular git commit citable?"
objectives:
  - "Learn how to generate a doi for particular git commit"
keypoints:
  - "By linking Zenodo with a github tag we get a permanent DOI for a particular version of history."
---

## Citing code

One of the major benefits of using code for data analysis is reproducibility and enabling transparency about the way data was manipulated and analysed. Using a version control system like git takes that to another level by enabling the sharing of the history of the code, as well allowing collaboration on code. 

But because by its very nature the code may change over time, it's necessary to be able to point to a particular point of history when we want to cite the code, such as in a research paper. 

> ## Discussion
> The old way of sharing a particular version of code was to upload a text file as supplementary material.
> - What are some of the shortcomings of this approach?
> - With your new expertise with git, what would be a better way?
{: .discussion}



## Referring to particular commits

To enable us to accurately refer to a particular commit, we need a label for it. The hash strings that git generates could work, but they are unwieldy. Branch pointers don't work because they stay at the tip of each branch.

Instead, git provides tags.

Tags are pointers that refer to a specific commit, and never move.

> ## Challenge
>
> - In the [vizualise git environment](http://git-school.github.io/visualizing-git/#free) have a go at creating tags.
> - Use `git tag [tagname]`
> - Create a tag, then add some commits. What happens to the different pointers?
> - Make some more tags, then practice doing `git checkout [tag]`
{: .challenge}

## Making tags static with github and Zenodo

While tags are really useful to point to a particular commit, they don't give a single access point - for example they exist in all copies of the repository, and there doesn't have to be a publicly accessible copy. For a citation, that's not much good.

Thankfully, github, in partnership with Zenodo, has a system to provide Digital Object Identifiers for particular tags. 

> ## Challenge
>
> - Create a new local repository
> - Make a few commits, and add a tag.
> - Create a new remote repository under your username at github.com, and set it as the remote for your local repository.
> - Use `git push origin [tag]` to add the tag you created locally to the remote
> Follow the guide at [https://guides.github.com/activities/citable-code/](https://guides.github.com/activities/citable-code/) to generate a DOI for that tag.
{: .challenge}
