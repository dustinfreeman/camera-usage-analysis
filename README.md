# Let's Analyze Our Smartphone Camera Usage Habits

This is a set of scripts to run on your computer, which analyze at your entire photo library to explore the **timing** of when you took photos. I'm doing this to explore my theory that smartphone photo-taking happens in "sessions", where, once your phone is out of your pocket/purse/backpack, you're usually taking more than one photo.

<table width="100%">
  <tr><img src="./photos_over_time.png" width="100%"/></tr>
  <tr>
    <td><img src="./photos_per_day_distribution.png" width="100%"/></td>
    <td><img src="./photos_per_session_distribution.png" width="100%"/></td>
  </tr>
</table>

## Identify Your Photo Library Folder Location

In Mac Photos app: `~/Pictures/Photos Library.photoslibrary/originals/`

On my Windows PC, in Dropbox `~/Dropbox/Camera Uploads/`

The scripts we'll run below will be able to see photos even if they're in subfolders.

First, use a shell open to this folder location, i.e.

```
$ cd <your photo library's directory>
```

## Get the list of all photos

```
$ find . -name "*.jpg" -o -name "*.jpeg" -o -name "*.heic" >> ~/my-photo-library-list.txt
```

NOTE: .png isn't included here on purpose, as those are typically screenshots.

Check how many photos the script found:

```
$ cat ~/my-photo-library-list.txt | wc -l
   23124
```

For example, I have 23k photos in my Mac library, from 2020-2024

## Get the photos' creation dates

You will need `exiftool` to read photo metadata.
On Mac, you can install this with `brew install exiftool`

The following takes a while to run. I found it takes about 1 minute per 1k photos in your library.

```
$ cat ~/my-photo-library-list.txt | xargs -I{} exiftool -time:all -G1 -a -s {} | grep SubSecCreateDate >> ~/my-photos-creationdate.txt
```

# Share with me!

Send the output `~/my-photos-creationdate.txt`, to me. Email works: dustin.freeman@gmail.com

To clean up, you can delete the two files we generated: `~/my-photo-library-list.txt` and `~/my-photos-creationdate.txt`

## Privacy Considerations

The output you share with me will have the timestamp and time zone of every file you've taken. It's fair if you feel uncomfortable sharing this private info. Read on for details...

Data Usage: All I'm doing with your timestamps is generating the plots "Photo Count Per Day" and "Sessions with a Given Number of Photos". I will save these images for comparison, and I will then delete the list of timestamps you sent. The plots generated will not be associated with your name anywhere (other than in my head, if I remember personally).

I use this [Jupyter Notebook](./photo-session-finder.ipynb) to generate the plots. You can run this yourself and send me your plots if you'd prefer.
