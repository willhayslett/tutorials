{{#template name="react-step09"}}

# Security with methods

Before this step, any user of the app could edit any part of the database. This might be okay for very small internal apps or demos, but any real application needs to control permissions for its data. In Meteor, the best way to do this is by declaring _methods_. Instead of the client code directly calling `insert`, `update`, and `remove`, it will instead call methods that will check if the user is authorized to complete the action and then make any changes to the database on the client's behalf.

### Removing `insecure`

Every newly created Meteor project has the `insecure` package added by default. This is the package that allows us to edit the database from the client. It's useful when prototyping, but now we are taking off the training wheels. To remove this package, go to your app directory and run:

```bash
meteor remove insecure
```

If you try to use the app after removing this package, you will notice that none of the inputs or buttons work anymore. This is because all client-side database permissions have been revoked. Now we need to rewrite some parts of our app to use methods.

### Defining methods

First, we need to define some methods. We need one method for each database operation we want to perform on the client. Methods should be defined in code that is executed on the client and the server - we will discuss this a bit later in the section titled _Optimistic UI_.

{{> DiffBox step="9.2" tutorialName="simple-todos-react"}}

Now that we have defined our methods, we need to update the places we were operating on the collection to use the methods instead:

{{> DiffBox step="9.3" tutorialName="simple-todos-react"}}

{{> DiffBox step="9.4" tutorialName="simple-todos-react"}}

Now all of our inputs and buttons will start working again. What did we gain from all of this work?

1. When we insert tasks into the database, we can now securely verify that the user is logged in, that the `createdAt` field is correct, and that the `owner` and `username` fields are correct and the user isn't impersonating anyone.
2. We can add extra validation logic to `setChecked` and `deleteTask` in later steps when users can make tasks private.
3. Our client code is now more separated from our database logic. Instead of a lot of stuff happening inside our event handlers, we now have methods that can be called from anywhere.

{{> step09OptimisticUI}}

{{/template}}
