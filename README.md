#What Does It Do?

I hate writing Display attributes.  If the client or end-user ever wants to change a field name, two places need to be edited.  While they may be close, one can often be overlooked.

The view model member name is usually descriptive enough to describe it.  So how does this project solve this problem?

ASP.NET MVC has a back-end class that aggregates all of the ComponentModel annotations into one object.  The aggregation is done whenever a view is called, and it is performed on the model class of that view.  When the view is being rendered, it then uses that object to determine the outputs of the various HTML Helper extension methods like Html.LabelFor, Html.DisplayFor, and Html.EditorFor.

This project provides an extension to the back-end class that does the aggregating.  Basically, the extension takes the member names of the model, and breaks them up into human-readable labels.  The process is very straightforward.

1. If a Display annotation exists for the member, use it with no modifications, otherwise continue with the following steps.
2. Break it up based on the stylistic convention - camel case or underscore delimited - into a list of words.
3. Capitalize the common acronyms, so acronyms like Xml and Id appear as XML and ID to the user.
4. Decapitalize the prepositions.
5. Decapitalize the articles.

#How Do I Use It?

Add the following to the Application_Start() method of the Global.asax file.

	ModelMetadataProviders.Current = new SmartDisplayNameMetaDataProvider();
	DescriptiveNameProvider.Current = new NameConventionUtility();
	
#Further Notes

The following keys can be used in the app settings of a configuration file to change how the labels are created. 
- **DecapitalizePrepositions**: If set it will decapitalize prepositions.  If not specified, the default value is true. 
- **DecapitalizeArticles**: If set it will decapitalize articles.  If not specified, the default value is true. 
- **CapitalizeAcronyms**: If set it will capitalize acronyms.  If not specified, the default value is true. 

The various special words (prepositions, articles, acronyms) may also be customized through the List<string> members of the NameConventionUtility.  The set of words considered acronyms is determined by the Acronyms member.  This is also the case for prepositions with the Prepositions member, and likewise for articles with the Articles member.

