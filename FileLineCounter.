package org.example;

import java.io.File;
import java.io.FileNotFoundException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
public class Main {
    public static void main(String[] args) throws Exception {
        // File.txt should contain list of file paths (each line = one file path)
        File file = new File("C:\\Users\\dhaya\\Filelist\\File.txt");
        Scanner sc = new Scanner(file);

        List<FileLineCounter> lineCounterArrayList = new ArrayList<>();
        long starttime=System.currentTimeMillis();
        System.out.println(Utility.formatMillisToTime(System.currentTimeMillis())+" Starting of Main");
        // Read file paths and start a thread for each
        while (sc.hasNextLine()) {
            String filepath = sc.nextLine();
            FileLineCounter fileLineCounter = new FileLineCounter(filepath);
            fileLineCounter.start();
            lineCounterArrayList.add(fileLineCounter);
        }
        sc.close();

        // Wait for all threads to finish
        for (FileLineCounter result : lineCounterArrayList) {
            result.join();
            System.out.println(Utility.trimFileName(result.getFilepath()) + " ---→ Contain [ " + result.getCount() + " Lines ] and "+"[ Total Execution time-->"+result.getExecutiontime()+"ms ]");
        }
        long endtime=System.currentTimeMillis();
        System.out.println(Utility.formatMillisToTime(System.currentTimeMillis())+" End of Main");
        System.out.println("Total Execution time -->  [ "+(endtime-starttime)+"ms ]");
    }
}
//Thread class to count Lines in a file
class FileLineCounter extends Thread {
    private final String filepath;
    private int count=0;
    long executiontime;

    FileLineCounter(String filepath) {
        this.filepath = filepath;

    }

    public long getExecutiontime() {
        return executiontime;
    }

    public int getCount() {
        return count;
    }

    public String getFilepath() {
        return filepath;
    }

    @Override
    public void run() {
        long starttime=System.currentTimeMillis();
        try (Scanner sc = new Scanner(new File(filepath))) {
        //    System.out.println(Utility.formatMillisToTime(System.currentTimeMillis())+" Processing [" + Utility.trimFileName(filepath) + " ] on thread [" + Thread.currentThread().getName() + " ]");
            while (sc.hasNextLine()) {
                String fileline = sc.nextLine();
                //Split lines in file
                String[] strings = fileline.split("\\.");
                for ( String line : strings ) {
                    // Count only nonempty line
                    if (!line.isEmpty()) {
                        count++;
                    }
                }
            }
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + filepath);
        }
     //    System.out.println(Utility.formatMillisToTime(System.currentTimeMillis())+" Completed Processing [" + Utility.trimFileName(filepath) + " ] on thread [" + Thread.currentThread().getName() + " ]");

        long endtime=System.currentTimeMillis();
        executiontime=endtime-starttime;
    }
}
// Utility class
class Utility {
    public static String formatMillisToTime(long millis) {
        long hours = (millis / (1000 * 60 * 60)) % 24;
        long minutes = (millis / (1000 * 60)) % 60;
        long seconds = (millis / 1000) % 60;
        long ms = millis % 1000;

        return String.format(" [[  %02d:%02d:%02d.%03d  ]]   --->  ", hours, minutes, seconds, ms);
    }
    //remove folder path and return filename
    public static String trimFileName(String filepath) {
        int  index= filepath.lastIndexOf('\\');
        return filepath.substring(index+1);
    }

}
