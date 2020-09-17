# Tweetify-Twitter-Spotify-
 Sentimental Analysis on Twitter and Spotify data, fetching live tweets from Twitter and finding shared Spotify music link in them

Project Title: 
Social Media and Music Streaming: Joining Twitter and Spotify APIs

Project Description:
This project integerates data retrieved from the Twitter API and the Spotify API,
joining these data sources on Spotify music tracks ids linked by Twitter users in
their tweets. This database is designed explicitly for future sentiment analysis
to assess the potential connection between audio features of a tweeted track
and the valence of tweet text/profile descriptions.

Requirements:
-twython (Twitter API client)
-spotipy (Spotify API client)
-spacy (text tokenization)
-json
-datetime
-regex
-pickle
-collections
-requests
-bs4 (web scraping)
-pandas
-numpy
-wordcloud
-matplotlib.pyplot
-PIL
-premium Twitter access tokens (paid account required)

Database Format:
Database is delivered as a .json; load_as_dataframe() can be used to load
data as a pd.DataFrame.

Variables Descriptions:

Twitter Data: 

User Variables:
 
	'User ID': "The integer representation of the unique identifier for this User."
			Example: '756201191646691328'
			Type: str

	'User Name': "The name of the user, as they’ve defined it. Not necessarily a person’s name."
			Example: 'Teye'
			Type: str

	'User Screenname': "The screen name, handle, or alias that this user identifies themselves with. 
			Screennames are unique but subject to change."
			Example: 'teyepiphany'
			Type: str

	'Member Since': "The UTC datetime that the user account was created on Twitter."
			Example: 'Tue Mar 02 18:17:10 +0000 2010'
			Type: str

	'User Profile Description': "The user-defined UTF-8 string describing their account."
			Example: 'Proud father, football & music fanatic from 
					Cardiff, Wales. Views are my own.'
			Type: str

	'Tokenized Profile Description': Tokenized version of User Profile Description.
			Example: ['proud',
 				'father',
 				'football',
 				'music',
 				'fanatic',
 				'from',
 				'cardiff',
 				'wales',
 				'views',
 				'are',
 				'my',
 				'own']
			Type: list<str>

	'User Location': "The user-defined location for this account’s profile. 
			Not necessarily a location, nor machine-parseable." 
			Example: 'Philadelphia, PA'
			Type: str

	'Follower Count': "The number of followers this account currently has."
			Type: int

	'Friend Count': "The number of users this account is following (AKA their “followings”)."
			Type: int

	'Status Count': "The number of Tweets (including retweets) issued by the user."
			Type: int

	
	'Favorites Count': "The number of Tweets this user has liked in the account’s lifetime."
			Type: int

	
Tweet Variables:

	'Tweet ID': "The integer representation of the unique identifier for this Tweet."
			Example: '1050118621198921728'
			Type: str

	'Tweet Text': "The actual UTF-8 text of the status update."
			Example: 'Vibes for today'
			Type: str

	'Tokenized Tweet Text': Tokenized version of the Tweet Text.
			Example: ['vibes',
 				'for',
 				'today']
			Type: list<str>

	'Creation Date Time': "UTC time when this Tweet was created."
			Example: 'Tue Mar 02 18:17:10 +0000 2010'
			Type: str

	'Retweet Count': 'Number of times this Tweet has been retweeted.'
			Type: int

	'Media URLs': list<dict>; keys = 'display_url', 'expanded_url', 'indices', 'url'

	'Coordinates' (if available): 'Represents the geographic location of this Tweet as reported by 
					the user or client application. The inner coordinates array is 
					formatted as geoJSON (longitude first, then latitude).'
			Example: {'type': 'Point', 'coordinates': [-2.11, 57.1526]}
			Type: dict

	'Place' (if available): 'When present, indicates that the tweet is associated 
				(but not necessarily originating from) a Place.'
			Example: {'id': '006523c50dfe9086',
 				'url': 'https://api.twitter.com/1.1/geo/id/006523c50dfe9086.json',
 				'place_type': 'city',
 				'name': 'Quezon City',
 				'full_name': 'Quezon City, National Capital Region',
 				'country_code': 'PH',
 				'country': 'Republic of the Philippines',
 				'contained_within': [],
 				'bounding_box': {'type': 'Polygon',
  				'coordinates': [[[120.989705, 14.5893763],
    				[121.1357656, 14.5893763],
    				[121.1357656, 14.7766484],
    				[120.989705, 14.7766484]]]},
 				'attributes': {}}
			Type: dict
	

Spotify Data:

Track Variables:

	'track name': "The name of the track."
			Example: 'Never Leave Me (feat. Joe Janiak)'
			Type: str

	'track artists': "The artists who performed the track."
			Examle: ['Avicii','Joe Janiak']
			Type: list<str>

	'track album': "The album on which the track appears."
			Example: 'TIM'
			Type: str

	'track release date': "The date the album was first released, for example 1981. 
			Depending on the precision, it might be shown as 1981-12 or 1981-12-15."
			Type: str

Audio Feature Variables:

	'danceability': "Danceability describes how suitable a track is for dancing 
			based on a combination of musical elements including tempo, 
			rhythm stability, beat strength, and overall regularity. 
			A value of 0.0 is least danceable and 1.0 is most danceable." 
			Type: float
	
	'energy': "Energy is a measure from 0.0 to 1.0 and represents a perceptual measure 
			of intensity and activity. Typically, energetic tracks feel fast, 
			loud, and noisy. For example, death metal has high energy, while 
			a Bach prelude scores low on the scale. Perceptual features contributing 
			to this attribute include dynamic range, perceived loudness, timbre, 
			onset rate, and general entropy."
			Type: float

	'key': "The estimated overall key of the track. Integers map to pitches using standard 
		Pitch Class notation . E.g. 0 = C, 1 = C♯/D♭, 2 = D, and so on. 
		If no key was detected, the value is -1."
			Type: int
	
	'loudness': "The overall loudness of a track in decibels (dB). Loudness values are 
			averaged across the entire track and are useful for comparing relative 
			loudness of tracks. Loudness is the quality of a sound that is the 
			primary psychological correlate of physical strength (amplitude). 
			Values typical range between -60 and 0 db."
			Type: float

	'mode': "Mode indicates the modality (major or minor) of a track, the type of scale 
		from which its melodic content is derived. Major is represented by 1 and minor is 0."
			Type: int


	'speechiness': "Speechiness detects the presence of spoken words in a track. 
			The more exclusively speech-like the recording 
			(e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value. 
			Values above 0.66 describe tracks that are probably made entirely of spoken words. 
			Values between 0.33 and 0.66 describe tracks that may contain both music and speech, 
			either in sections or layered, including such cases as rap music. 
			Values below 0.33 most likely represent music and other non-speech-like tracks."
			Type: float

	'acousticness': "A confidence measure from 0.0 to 1.0 of whether the track is acoustic. 
			1.0 represents high confidence the track is acoustic."
			Type: float

	'instrumentalness': "Predicts whether a track contains no vocals. 
				“Ooh” and “aah” sounds are treated as instrumental in this context. 
				Rap or spoken word tracks are clearly “vocal”. 
				The closer the instrumentalness value is to 1.0, 
				the greater likelihood the track contains no vocal content. 
				Values above 0.5 are intended to represent instrumental tracks, 
				but confidence is higher as the value approaches 1.0."
			Type: float

	'liveness': "Detects the presence of an audience in the recording. 
			Higher liveness values represent an increased probability that the track was performed live. 
			A value above 0.8 provides strong likelihood that the track is live. 
			Type: float

	'valence': "A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. 
			Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), 
			while tracks with low valence sound more negative (e.g. sad, depressed, angry)."
			Type: float

	'tempo': "The overall estimated tempo of a track in beats per minute (BPM). 
		In musical terminology, tempo is the speed or pace of a given piece 
		and derives directly from the average beat duration."
			Type: float

	'duration_ms': "The duration of the track in milliseconds."
			Type: int

	'time_signature': "An estimated overall time signature of a track. 
			The time signature (meter) is a notational convention to specify 
			how many beats are in each bar (or measure)."
			Type: int

	'id': Unique track identifier given by Spotify API.
			Example: '7zGEU6BXl2c4TxpzIAr7BI'
			Type: str

	'uri': "The Spotify URI for a track."
			Example: 'spotify:track:7zGEU6BXl2c4TxpzIAr7BI'
			Type: str

	'track_href': "A link to the Web API endpoint providing full details of the track."
			Example: 'https://api.spotify.com/v1/tracks/7zGEU6BXl2c4TxpzIAr7BI'
			Type: str

	'analysis_url': "An HTTP URL to access the full audio analysis of this track. 
			An access token is required to access this data."
			Example: 'https://api.spotify.com/v1/audio-analysis/7zGEU6BXl2c4TxpzIAr7BI'
			Type: str

Challenges:

	1. Pagination of Twitter API and successfully adjusting Twython query parameters
		Description: The Twitter API response contains a 'search_metadata' key.
		This key contains a 'max_id' key.

	
2. Building a specialized scraper depending on the type of Spotify media linked 
		(e.g., artist vs. playlist vs. single track)

	
3. Interruptions in long data downloads (addressed w/ batching)
