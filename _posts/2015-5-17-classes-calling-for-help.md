---
layout: post
title: Hidden classes calling for help
---

Recently, I learned that in past I have used

1. case statements  
  to call specific methods based on the input( i.e. some string ) to case statement.

2. respond_to?  
   calling specific methods after checking if the current object responds to a message.

3. kind_of?  
   checking the class type and then calling a specific method in that class

  In all the above 3 cases, what is really happening is that there is _hidden_ class calling out for help to be extracted out.

  Following example will make it clear.

  The following code explain the first scenario.  

  Consider a FeedProcessor, which processes a feed and extracts different type of videos.
  In the following code, the FeedProcessor knows to much about the video extraction logic.

  ```
  class FeedProcessor
    def initialize(extractors)
      @extractor = extractors
    end

    def process(feed)
      .
      .
      extract_video(video_url)
      .
      .
    end

    def extract_video(video_url)
      case video_url
      when YOUTUBE_REGEX
        extract_youtube_video(video_url)
      when VIMEO_URL
        extract_vimeo_video(video_url)
      when POCKET_URL
        extract_pocket_video(video_url)
      end
    end

    def extract_youtube_video
      # youtube video extraction
    end

    def extract_vimeo_video
      # vimeo video extraction
    end

    def extract_pocket_video
      # pocket video extraction
    end
  end
  ```
  Instead of branching on the type of video url, pass in the **video extractors**( _duck type_ ). A video extractor knows what url it understands, hence can go ahead and extract the video. That logic can be nicely encapsulated inside the appropriate extractor.

  ```
  class FeedProcessor
    def initialize(extractors)
      @extractor = extractors
    end

    def process(feed)
      .
      .
      extract_video(video_url)
      .
      .
    end

    def extract(extractors)
      extractors.each do |extractor|
        extract(video_url)
      end
    end
  end

  class YouTubeVideoExtractor
    def extract(video_url)
      # examine the video_url and
      # if it is YouTube url then extract video
    end
  end

  class VimeoVideoExtractor
    def extract(video_url)
      # examine the video_url and
      # if it is vimeo url then extract video
    end
  end

  class PocketVideoExtractor
    def extract(video_url)
      # examine the video_url and
      # if it is pocket url then extract video
    end
  end  

  ```

  So whenever you see case statement to switch and execute methods with names following the pattern **extract_XXXXXX**, you know there is hidden duck type.

  Same argument goes for using respond\_to? and kind\_of?. You shouldn't use these test. It is better to use define classes.
