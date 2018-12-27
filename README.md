# Zip Support

This is a [Codename One](https://www.codenameone.com) library for Zip support based on the code from the [zipme](https://sourceforge.net/projects/zipme/) project. As such it carries a modified GPL license similar to the classpath exception but not similar enough to include in the core distribution.

This library can be used to enable ZIP functionality in Codename One applications.

You can download an install this library from [here](https://github.com/codenameone/ZipSupport/blob/master/ZipSupport.cn1lib?raw=true).


The sample code below demonstrates zip and unzip usage.

````java
public void Unzip(InputStream is_zipFile, String str_dirDest) 
 {
    InputStream is;
    try 
    {
        is = is_zipFile;
        ZipInputStream zipStream = new ZipInputStream(is);
        ZipEntry entry;

        // create a buffer to improve copy performance later.
        byte[] buffer = new byte[2048];
        System.out.println("TRYING TO UZIP: ");

        while ((entry = zipStream.getNextEntry()) != null) 
        {
            FileSystemStorage fs = FileSystemStorage.getInstance();
            String str_name = entry.getName();
            String dir;
            String str_outdir = FileSystemStorage.getInstance().getAppHomePath();
            //FileOutputStream fileoutputstream;
            File newFile = new File(str_outdir, str_name);
            boolean overwrite = false;
            if (str_outdir.length() > 0) 
            {
                str_outdir = str_outdir + "/" + str_dirDest;
            }

            //extractFile(zin, outdir, name);
            String outpath = str_outdir + "/" + entry.getName();
            OutputStream output = null;

            try 
            {
                if (entry.isDirectory()) 
                {

                    fs.mkdir(str_outdir + "/" + str_name);
                    entry = zipStream.getNextEntry();
                    continue;
                } 
                else 
                {
                    File file = new File(str_outdir + File.separator + str_name);
                    File parent = file.getParentFile();
                    if (!parent.exists()) 
                    {
                        parent.mkdirs();
                    }

                }

                System.out.println("UNZIPPING:- " + str_name);
                output = FileSystemStorage.getInstance().openOutputStream(outpath);
                int len = 0;
                while ((len = zipStream.read(buffer)) > 0) 
                {
                    output.write(buffer, 0, len);
                }
            } 
            catch (Exception e) 
            {
                e.printStackTrace();
                //Dialog.show("Unzipping Error!", ""+e, "Ok", null);
            } 
            finally 
            {

                if (output != null) 
                {
                    output.close();
                }
            }
        }
    } catch (IOException ex) {
        Log.p(ex.getMessage(), 0);

    }
}
````
