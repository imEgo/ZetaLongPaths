# Zeta Long Paths

A .NET library to access files and directories with more than 260 characters length.

[![Build state](https://travis-ci.org/UweKeim/ZetaLongPaths.svg?branch=master)](https://travis-ci.org/UweKeim/ZetaLongPaths "Travis CI build status")

## Introduction

This article describes a library that provides several classes and functions to perform basic functions on file paths and folder paths that are longer than the "MAX_PATH" limit of 260 characters.

## Background

All .NET functions I came across that access the file system are limited to file paths and folder paths with less than 260 characters. This includes most (all?) of the classes in the [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx) namespace like e.g. the [System.IO.FileInfo](http://msdn.microsoft.com/en-us/library/system.io.fileinfo.aspx) class.

Since I was in the need to actually access paths with more than 260 characters, I searched for a solution. Fortunately a solution exists; basically you have to P/Invoke Win32 functions that allow a special syntax to prefix a file and allow it then to be much longer than the 260 characters (about 32,000 characters).

## The library

So I started writing a very thin wrapper for the functions I required to work on long file names.

These resources helped me finding more:

  * "[Long Paths in .NET](http://blogs.msdn.com/bclteam/archive/2007/02/13/long-paths-in-net-part-1-of-3-kim-hamilton.aspx)" on the BCL Team Blog.
  * [pinvoke.net](http://pinvoke.net/) for finding signatures of Win32 functions.
  * [Using long path syntax with UNC](http://msdn.microsoft.com/en-us/library/aa365247.aspx), MSDN.

I started by using several functions from the BCL Team blog postings and added the functions they did not cover but which I needed in my project.

Currently there are the following classes:

  * `ZlpFileInfo` - A class similar to [System.IO.FileInfo](http://msdn.microsoft.com/en-us/library/system.io.fileinfo.aspx) that wraps functions to work on file paths.
  * `ZlpDirectoryInfo` - A class similar to [System.IO.DirectoryInfo](http://msdn.microsoft.com/en-us/library/system.io.directoryinfo.aspx) that wraps functions to work on folder paths.
  * `ZlpIOHelper` - A set of static functions to provide similar features as the `ZlpFileInfo` and `ZlpDirectoryInfo` class but in a static context.
  * `ZlpPathHelper` - A set of static functions similar to [System.IO.Path](http://msdn.microsoft.com/en-us/library/system.io.path.aspx) that work on paths.

## Using the code

The project contains some unit tests to show basic functions.

If you are familiar with the [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx) namespace, you should be able to use the classes of the library.

For example to get all files in a given folder path, use the following snippet:

    var folderPath = new ZetaDirectoryInfo( @"C:\My\Long\Folder\Path" );
	 
    foreach ( var filePath in folderPath.GetFiles() )
    {
        Console.Write( "File {0} has a size of {1}", 
            filePath.FullName, 
            filePath.Length );
    }

## Other libraries

Beside this library, there are other libraries available for accessing longer paths:

- [Long Path](http://bcl.codeplex.com/releases/view/42783) from the BCL CodePlex library.
- [Delimon.Win32.IO Library](https://gallery.technet.microsoft.com/DelimonWin32IO-Library-V40-7ff6b16c) from the Microsoft TechNet Gallery.
- [QuickIO.NET](http://quickio.net/)
- [AlphaFS](https://github.com/alphaleonis/AlphaFS)

Personally, I've used none of these libraries. When I started developing this library either none of the other libraries existed or I have poorly searched.

According to user comments, the _Long Path_ library is rather restricted in terms of functionality; the _Delimon_ library is apparently much more powerful than my library.

## Summary

This article quickly introduced a library to deal with long file names when accessing files and folders.

Please note that the library currently is limited in the number of provided functions. I will add more functions in the future, just tell me which you require.

I'm using the library in several widely used real-life projects like our [CMS](http://www.zeta-producer.com), our [Test Management tool](http://www.zeta-test.com) or our [Large File Uploader](https://www.zeta-uploader.com), so the library should be rather stable and reliable.

## History

  * *2014-09-18* - Added functions like `MoveFileToRecycleBin()` to delete files and folders by moving them to the recycle bin.
  * *2014-06-25* - First release to GitHub. Also available at [The Code Project](http://www.codeproject.com/Articles/44904/Zeta-Long-Paths).
  * *2012-12-21* - Added an [NuGet package](http://nuget.org/packages/ZetaLongPaths).
  * *2012-09-20* - Some very few methods added. Stability release.
  * *2012-08-10* - Added several new methods.
  * *2011-10-11* - Fixed an issue inside _ZlpFileInfo.Exists_.
  * *2011-01-31* - Added functions _MoveFile_ and _MoveDirectory_.
  * *2010-03-24* - Added functions to get file owner, creation time, last access time, last write time.
  * *2010-02-16* - Maintenance release.
  * *2009-11-25* - First release to [CodePlex.com](https://zetalongpaths.codeplex.com).

(This is not a complete history; only the milestones are noted)
