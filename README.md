# easy-content-providers
Work easy with Android built-in and custom content providers data + Stetho extension + Sample app

[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-easy--content--providers-brightgreen.svg?style=flat)](http://android-arsenal.com/details/1/2334)

## Here, About the Libraries
- [Android Providers](#android-providers)<br>
	Simple API for all Android built-in providers. Get, Update, Delete, Insert.

	```
	compile 'me.everything:providers-android:1.0.1' 
	```

- [Custom Model and Provider](#custom-provider)<br>
	Simple ORM annotations for your own model and provider.

	```
	compile 'me.everything:providers-core:1.0.0' 
	```

- [Stetho Extension](#stetho-extension)<br>
	[Stetho](https://github.com/facebook/stetho) extension library to view all content providers data in Chrome Developer Tool.

	```
	compile 'me.everything:providers-stetho:1.0.1' 
	```

- [Sample App](#sample-app)<br>
	Sample app that presents all android providers and shows custom model implementation and has the Stetho setup.


## Why and when
Many times we experiment new features, new apps, new ideas or we just code through hackathon, or we want to debug and test results. In such cases, we need tools to get what we want in a minimum time. So, this is the idea of these libraries. We can get all android stored data very easily and we can debug results through Chrome dev tools (based on stetho implementation). 

Now, if you look for very high performance, then check if these libs meet your needs. Because, reflection is used here, and some additional operations to make ORM implementation generic. Also, if you don't define the exact columns you want, the mapping will happen on all columns (select * from ...). Sometimes, this is not what you want.

## Android Providers

```
dependencies {
    compile 'me.everything:providers-android:1.0.1'
}
```

- Calendars
	- Events
	- Instances
	- Reminders
	- Attendees
- Contacts
- Call logs
- Telephony
	- SMS
	- MMS
	- Conversations
	- Threads
	- Carriers
- Media (external + internal)
	- Files
	- Images
	- Videos
	- Audios
	- Albums
	- Artists
	- Genres
	- Playlists
- Browser 
	- Bookmarks
	- Searches
- Dictionary

#### Calendars, Events, Instances, Attendees, Reminders

``` java
CalendarProvider calendarProvider = new CalendarProvider(context);
```

Required permission:
``` xml
<uses-permission android:name="android.permission.READ_CALENDAR"/>
```

**Calendar**
- `calendarProvider.getCalendars();` - Get all calendars
- `calendarProvider.getCalendar(calendarId);` - Get calendar by id
- `calendarProvider.update(calendar);` - Update calendar

**Event**
- `calendarProvider.getEvents(calendarId);` - Get all events by calendar id
- `calendarProvider.getEvent(eventId);` - Get event by event id
- `calendarProvider.update(event);` - Update event

**Instance**
- `calendarProvider.getInstances(begin, end);` - Get all instances by date range
- `calendarProvider.getInstances(eventId, begin, end);` - Get all instances by date range of event
- `calendarProvider.update(instance);` - Update instance

**Attendee**
- `calendarProvider.getAttendees(eventId);` - Get all attendees of event
- `calendarProvider.update(instance);` - Update instance

**Reminder**
- `calendarProvider.getReminders(eventId);` - Get all reminders of event
- `calendarProvider.update(reminder);` - Update reminder

<br>

#### Contacts

``` java
ContactsProvider contactsProvider = new ContactsProvider(context);
```

Required permission:
``` xml
<uses-permission android:name="android.permission.READ_CONTACTS"/>
```

**Contact**
- `contactsProvider.getContacts();` - Get all contacts

<br>

#### Calls Log

``` java
CallsProvider callsProvider = new CallsProvider(context);
```

Required permission:
``` xml
<uses-permission android:name="android.permission.READ_CALL_LOG"/>
```

**Call**
- `callsProvider.getCalls();` - Get all calls

<br>

#### Telephony, SMS, MMS, Conversations, Threads, Carriers

``` java
TelephonyProvider telephonyProvider = new TelephonyProvider(context);
```

Required permission:
``` xml
<uses-permission android:name="android.permission.READ_SMS"/>
```

**Sms**
- `telephonyProvider.getSms(filter);` - Get all sms(es) by filtering (inbox, sent, ...)

**Mms**
- `telephonyProvider.getMms(filter);` - Get all mms(es) by filtering (inbox, sent, ...)

**Conversation**
- `telephonyProvider.getConversations();` - Get all conversations

**Thread**
- `telephonyProvider.getThreads();` - Get all threads

**Carrier**
- `telephonyProvider.getCarriers();` - Get all carriers

<br>

#### Media

``` java
MediaProvider mediaProvider = new MediaProvider(context);
```

Required permission:
``` xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

**File**
- `mediaProvider.getFiles(storage);` - Get all files

**Image**
- `mediaProvider.getImages(storage);` - Get all images

**Video**
- `mediaProvider.getVideos(storage);` - Get all videos

**Audio**
- `mediaProvider.getAudios(storage);` - Get all audios

**Albums**
- `mediaProvider.getAlbums(storage);` - Get all albums

**Artists**
- `mediaProvider.getArtists(storage);` - Get all artists

**Genres**
- `mediaProvider.getGenres(storage);` - Get all genres

**Playlists**
- `mediaProvider.getPlaylists(storage);` - Get all playlists

<br>

#### Browser 

``` java
BrowserProvider browserProvider = new BrowserProvider(context);
```

Required permission:
``` xml
<uses-permission android:name="com.android.browser.permission.READ_HISTORY_BOOKMARKS"/>
```

**Bookmark**
- `browserProvider.getBookmarks();` - Get all bookmarks

**Search**
- `browserProvider.getSearches();` - Get all searches

<br>

#### Dictionary

``` java
DictionaryProvider dictionaryProvider = new DictionaryProvider(context);
```

Required permission:
``` xml
<uses-permission android:name="android.permission.READ_USER_DICTIONARY"/>
```

**Word**
- `dictionaryProvider.getWords();` - Get all words

<br>

#### Prefer Collection or Cursor

Every response is wrapped with the `Data` object. You can always choose if you want the list or the cursor.
To get a `List` with the right model type:
``` java
List<Calendar> calendars = data.getList();
```

To get a `Cursor`:
``` java
Cursor cursor = data.getCursor();
```

<br>

----

## Custom Provider

You can create your own model, based on your own content provider. Use annotations to map your table schema to your model. This will give you the same power of simplicity as for android providers. In fact, android providers has the same technics.

```
dependencies {
    compile 'me.everything:providers-core:1.0.0'
}
```

#### Model Mapping 
By using annotations we map model members to table columns and define some basic ORM mapping. 

To make it work extend you model by `Entity` class.

``` java
public class Post extends Entity  {
	...
}
```

And annotate your fields by using next options:

* Ignore mapping
	``` java
	@IgnoreMapping
	private String someField; // field you want to ignore
	```

* Map **Integer** to **int**, **Text** to **String**, 
	``` java
	@FieldMapping(
		columnName = "_id", 
		physicalType = FieldMapping.PhysicalType.Int
	)
    public int id;

    @FieldMapping(
    	columnName = "title", 
    	physicalType = FieldMapping.PhysicalType.String
    )
    public String title;
    ```

* Map **Blob** to **byte[]**
	``` java
	@FieldMapping(
		columnName = "thumbnail", 
		physicalType = FieldMapping.PhysicalType.Blob
	)
    public byte[] thumbnail;
    ```

* Map **Integer** to **boolean**
	``` java
	@FieldMapping(
		columnName = "is_owner", 
		physicalType = FieldMapping.PhysicalType.Int, 
		logicalType = FieldMapping.LogicalType.Boolean
	)
    public boolean isOwner;
    ```

* Map **Integer** to **long**
	``` java
	@FieldMapping(
		columnName = "updated_at", 
		physicalType = FieldMapping.PhysicalType.Int, 
		logicalType = FieldMapping.LogicalType.Long
	)
    public long updatedAt;
    ```

* Map **Integer** to **Enum**
	``` java
	@FieldMapping(
		columnName = "post_type", 
		physicalType = FieldMapping.PhysicalType.Int, 
		logicalType = FieldMapping.LogicalType.EnumInt
	)
    public PostType postType;

    public enum PostType implements EnumInt {
		LINK(1),
		VIDEO(2);

		int val;

		PostType(int val) {
			this.val = val;
		}

		public static PostType fromVal(int val) {
			for (PostType postType : values()) {
				if (postType.val == val) {
					return postType;
				}
			}
			return null;
		}
	}
    ```

<br>

----

## Stetho Extension
It is possible to present all providers data in Chrome Dev Tool by extending Stetho lib. You don't need all stuff above, if this is the only thing you want.

```
compile 'me.everything:providers-stetho:1.0.1' 
```

**Setup Stetho**<br>
Setup in default mode + this extension with all Android built-in providers
``` java
public class MainApplication extends Application {

    public void onCreate() {
        super.onCreate();

		ProvidersStetho providersStetho = new ProvidersStetho(context);
		providersStetho.enableDefaults();

		Stetho.initialize(Stetho.newInitializerBuilder(context)
		    .enableDumpapp(Stetho.defaultDumperPluginsProvider(context))
		    .enableWebKitInspector(providersStetho.defaultInspectorModulesProvider())
		    .build());	

	}

}
```

And this is how it looks:

![Screenshot](https://github.com/EverythingMe/easy-content-providers/wiki/images/stetho-providers.png)

**Add your own providers**<br>
And of course, you can see your own providers by registring them. In our example (Check sample app):
``` java
... 
providersStetho.registerProvider("provider-custom", "posts", new ProvidersStetho.QueryExecutor<Post>() {
    @Override
    public Data<Post> onQuery(String query) {
        PostsProvider provider = new PostsProvider(getApplicationContext());
        return provider.getPosts();
    }
});	
...
```

And this is how it looks:

![Screenshot](https://github.com/EverythingMe/easy-content-providers/wiki/images/stetho-providers-custom.png)

**SQL Query - Important Notes**

- Once you select a table, it might take few seconds until the result is shown. It's just go all the way to provider and back.

- If you decide to run query, the only supported query is `select * from "tablename"`.

- For `provider-calendar` tables you can select from `events`, `instances`, `reminders`, `attendees` only in the next way (until proper query resolver solution will be added):

	- `#events:id=3` - select from events where calendar id = 3
	- `#instances:id=100` - select from instances where event id = 100
	- `#reminders:id=100` - select from reminders where event id = 100
	- `#attendees:id=100` - select from attendees where event id = 100

	![Screenshot](https://github.com/EverythingMe/easy-content-providers/wiki/images/stetho-providers-sql.png)

<br>

----

## Sample App

- Sample app has all content provider examples with screen for each provider data
- Stetho is there, so you can inspect it

	![Screenshot](https://github.com/EverythingMe/easy-content-providers/wiki/images/sample-app.png)

## Author
[Roman Kushnarenko - sromku](https://github.com/sromku)

## License
Apache License 2.0