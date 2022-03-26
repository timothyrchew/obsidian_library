getting access to auth0
- how do they handle testing out users and user flow in dev? how can i test out, mock the new user flow?

email verification feature specific (qualify: in my opinion frontend, much more than backend leaves a lot more to convention and style, so im going to ask some pretty basic questions just to calibrate my approach to this team)
- does he understand this as a pinned, global alert?
- this is less so the case with this feature, but what type of work involves designs vs engineer just using common sense? do they have ux / design team?
- new banner component with the uncontrolled reactstrap alert vs re-using existing alert (and having to update all current uses to account for additional color.
- boostrap philisophy. use whenver possible? how averse to custom positioning? i find that more custom positining can get pretty tough with bootstrap classes. in this case would it be ok to write custom css?
- double check my approach for where the banner is added (AppLayout.js)
- i assume unit testing for this. would they also include cypress e2e testing for something like this? anything i need to know about the cypress approach? (ive done e2e but not working specifically with cypress)

overall conventions
- what are the aims for mobile responsiveness? what is the use for redux for setting classnames at the app level? seems like pretty complicated way to manage responsiveness?
- talking more about where they put styling for something like container to pin to top of the page
