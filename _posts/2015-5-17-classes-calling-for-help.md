---
layout: post
title: Hidden classes calling for help
---

Recently, I learned that I have in past used
1. case statements  
  call specific methods based on the input to case statement.

2. respond_to?

  calling class specific methods after checking if the current object responds to a message.

3. kind_of?

  checking the class type

  In all the above 3 cases, what is really happening is that there is _hidden_ class calling out for help to be extracted out.

  Following example will make it clear. The following ocde explain the first scenario.

  ´´´
  
  class FeedProcessor
    attr_accessor
    def process
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
  ´´´
