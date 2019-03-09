
Welcome!


## What Is ubqt?

  ubqt is a [pao](http://tiamat.tsotech.com/pao) inspired playground, using filesystem-based program state representations to decouple the runtime state of a program, from any particulars of how it is presented to the end user.

## Why?

  The future is now. We have computers that are quite functionally everywhere; and the price to put a screen on an arbitrary wall, toaster, fridge, table, etc. is no longer as prohibitative as it was. The downside is that each of those screens is being used to display information in a manner which at best doesn't work for everyone, and at worst is completely unusable. 

## HTML

  This is in no small part due to the nature and ubiquity of HTML, which defines the document structure and theme authoritatively; any compliant view of an HTML document should look as it was designed to look.

## What's So Bad About That?

  The downside is that the content no longer matches well to the intent for many tasks, which are moving to web-based solutions in unprecidented numbers. A bespoke calendar service, which shows upcoming dates and appointments would have to be either positively master crafted to match well to the variety of devices, or leverage a stack of software with many millions of cycles of overhead, and many thousands of lines of code to bring edge cases when problems arise. This difficulty to provide a satisfactory experience is mirrored in the variety and quality of solutions for a specific task, where many offerings are incomplete and/or very limited in the scope of devices they'll correctly work on.

## Most Of My Stuff Works Fine

  Great! You're among the many satisfied individuals. For many, especially the differently abled, the experience can be arduous for even the most simple tasks. As more services are moving strictly to the above described method, the situation threatens to worsten.

  That being said, discovery of satisfactory services is difficult. Searching for applications on each platform you use, vetting their quality, and finding ways to integrate them with other services is thirsty work. 

## What Is Ubqt Gonna Do About It?

  Leveraging a ubqt network, adding a service that suits your needs is much more trivial; the overall state for a calendar of upcoming tasks is a set of lists, with links to the individual appointments - very trivial indeed!
The chance that you'll need a bespoke client and server side solution is lowered by ubqt. Many things can be wholly described via ubqt's structure, as many services are equally described via HTML.

## Alright. Alright. Where's The But?

-Behind you.-

  The downside for the average, happy user is as follows: services no longer describe the eventual user interface as fully as they do via HTML. It's inherently lossy - a pdf document will not render in the preferred font, the page layout semantics will not be honoured, and the images will not be center-justified or left/right justified as they present in the document.
  If this is something you simply cannot give up, please feel free to contact the author of this page, [halfwit](https://github.com/halfwit), and he will be happy to provide you a list of his favorite bespoke solutions for pixel-perfect document production; but please read on, there's another side to this coin.

## PAO

  In the very first paragraph, there was a link to an article about persistent application omnipresence. If you didn't chance to read it, a basic summary is that your applications follow you from device to device. That is to say, you can stop midway through typing an email on your desktop computer, have to leave unexpectedly, open your phone, and continue typing exactly where you left off. This is a feature of quite a few interesting services; but it is non-trivial to implement this well. Also, the author explains that this should be the default, for every service that they want. 

## Ubqt's Take On PAO

  ubqt facilitates many of the tenets of pao. In addition, it challenges that the interface should match the device it's drawn on - browsing the internet on your mobile phone circa 2008 was a terrible experience, and only with much work from millions of people did it ever improve. On the face of the problem, this is solved. Taking a step back, it's not even close to solved - introduce a new device, such as a smart glass, a wearable, a toaster; and the design is back to the stone ages, unusable at worst, and unideal at best. 

## Ubqt Clients

  Due to the trivial nature of implementing a ubqt client, much care and time can be spent marrying the client implementation to the device it is presented on. Due to the very simple interface to the underlying state, concerns about how arbitrary data will be presented can be assuaged, and the focus can be solely on presenting the data in the way that best makes sense. Your toaster is safe from @media queries, finally.

## Design Details

  Further reading about the underlying architectural details can be found here

 - Coming Soon!
