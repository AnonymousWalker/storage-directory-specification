# Set Transition diagram:
```
{LANGUAGE} -> {RESOURCE}

    {RESOURCE} -> {PROJECT}
               -> CONTENTS/{MEDIA}
               -> CONTENTS/{CONTAINER}

        {PROJECT} -> CONTENTS/{CONTAINER}
                  -> CONTENTS/{MEDIA}

            {CONTAINER} -> {MEDIA}

                {MEDIA: COMPRESSED} -> {QUALITY}
                {MEDIA: UNCOMPRESSED} -> {GROUPING}

                    {QUALITY} -> {GROUPING}

                        {GROUPING} -> FILENAME
```
Names in { } denote a set, with the possible values contained in that set being defined below. To use this state transition diagram, begin with the {LANGUAGE} set, print the value in the set, and follow to the next set that most accurately describes the data being inserted (or searched for). Step by step instructions can be found below as well as a sample walkthrough.

**NOTE: CONTENTS = just a directory named CONTENTS to signify the beginning of content (so that file formats do not overlap with possible project identifiers)**

## Set Definitions and Values:
 - {LANGUAGE} = language code from resource container (such as en)

 - {RESOURCE} = id from the dublin_core section of the resource container manifest (such as ulb)

 - {PROJECT} = project is the project id in the resource container manifest (such as book id)

 - {CONTAINER} = file extension of the container holding media (currently only tr)

 - {MEDIA} = file extension of the media file (such as mp3, wav, docx, jpg, png) 

 - {QUALITY} = Either hi or low (only for compressed file formats such as mp3, jpg, png)

 - {GROUPING} = what the media file contains (book, chapter, chunk, verse) for example chapter for audio files containing an entire chapter

 - FILENAME = the name of the file

**NOTE: When dealing with TR files, or other similar container files, the media format is the format (such as wav or mp3) of files inside the container (TR).**

## Media format examples (not exhaustive):
 - Compressed: mp3, mp4, ogg, m4a, 3gp, jpg, png, gif
 - Uncompressed: wav, flac, bmp, svg

# Instructions:
1. Begin with the language code followed by /
2. Add the identifier from the dublin_core section of the resource container manifest followed by /
3. If the file represents the entire resource container, add CONTENTS/ skip to step 5.
4. Add the identifier of the project followed by /CONTENTS/
5. If the media file is not inside a container format (such as tr) skip to step 7.
6. Add the file extension (without the .) of the container format followed by /
7. Add the file extension (without the .) of the media format followed by /
8. If the media format is an uncompressed format, skip to step 10.
9. Add the quality of the file (either "hi" for high quality or "low" for low quality) followed by /
10. Add "book", "chapter", "chunk", or "verse" based on how much content is grouped by the format, followed by /
11. Add the filename

**NOTE: Unless intentionally created for low bandwidth usage, quality for compressed formats should be considered "hi" by default.**

## Example:
#### English tr source audio of genesis where the tr file contains high quality mp3 files in chunks:

1. en/
2. en/ulb/
3. proceed to step 4.
4. en/ulb/gen/CONTENTS/
5. proceed to step 6.
6. en/ulb/gen/CONTENTS/tr/
7. en/ulb/gen/CONTENTS/tr/mp3/
8. proceed to step 9.
9. en/ulb/gen/CONTENTS/tr/mp3/hi/
10. en/ulb/gen/CONTENTS/tr/mp3/hi/chunk/
11. en/ulb/gen/CONTENTS/tr/mp3/hi/chunk/en_ulb_gen_chunk.tr

## Example fileystem:
```
en/  ulb/   gen/  CONTENTS/  mp3/   hi/    chunk/      
            |                |      |      chapter/
            |                |      |      verse/
            |                |      -----------------------------       
            |                |      low/   chunk/
            |                |      |      chapter/
            |                |      |      verse/
            |                |      -----------------------------
            |                ------------------------------------   
            |                wav/    chapter/
            |                |       chunk/
            |                |       verse/
            |                ------------------------------------   
            |                tr/     mp3/    hi/     chunk/      
            |                |       |       |       chapter/
            |                |       |       |       verse/            
            |                |       |       --------------------
            |                |       |       low/    chunk/
            |                |       |       |       chapter/
            |                |       |       |       verse/
            |                |       |       --------------------
            |                |       ----------------------------
            |                |       wav/      chapter/
            |                |       |         chunk/
            |                |       |         verse/
            |                |       ----------------------------
            |                ------------------------------------
            -----------------------------------------------------
            CONTENTS/   mp3/    /hi     chunk/      
            |           |       |       chapter/
            |           |       |       verse/
            |           |       ---------------------------------
            |           |       /low      chunk/
            |           |       |         chapter/
            |           |       |         verse/
            |           |       ---------------------------------
            |           wav/    chapter/
            |           |       chunk/
            |           |       verse/
            |           -----------------------------------------
            |           tr/     /mp3    hi/      chunk/      
            |           |       |       |        chapter/
            |           |       |       |        verse/
            |           |       |       -------------------------
            |           |       |       low/    chunk/
            |           |       |       |       chapter/
            |           |       |       |       verse/
            |           |       |       -------------------------
            |           |       wav/    chapter/
            |           |       |       chunk/
            |           |       |       verse/
            |           |       ---------------------------------
            |           -----------------------------------------
            -----------------------------------------------------
```
