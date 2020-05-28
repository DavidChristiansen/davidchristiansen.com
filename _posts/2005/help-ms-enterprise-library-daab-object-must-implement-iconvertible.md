---
ID: 224
post_title: 'Help! &#8212; MS Enterprise Library DAAB &#8211; Object must implement IConvertible'
post_name: >
  help-ms-enterprise-library-daab-object-must-implement-iconvertible
author: David
post_date: 2005-04-01 18:12:00
layout: post
link: >
  https://blog.davidchristiansen.com/2005/04/help-ms-enterprise-library-daab-object-must-implement-iconvertible/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>I am having real problems with the above error (which after searching on google seems to have been inherited from the old DAAB) when calling db.UpdateDataSet. <br><br>I am attempting to update a SQL database from a dataset with 8 or tables in them... under a transaction. <br><br>-----</p>
<p>Update:</p>
<p>This fault caught me several times and it transpires that it actually WAS&nbsp;down to an illegal casttyping.... <br>I use NullableTypes and was passing a NullableDateTime directly where what I should have been doing was something like Convert.ToDateTime(MyDateOfBirthAsNullableDateTime.toString()).</p>
<p>So there you go!</p>