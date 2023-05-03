Download Link: https://assignmentchef.com/product/solved-cs2030-lab-3-cruise-loaders
<br>



<h1>Task Content</h1>

<table width="680">

 <tbody>

  <tr>

   <td width="680"><strong>Cruise Loaders Topic Coverage</strong>InheritancePolymorphismMethod Overriding <a href="https://www.comp.nus.edu.sg/~cs2030/style/">CS2030 Java St</a><a href="https://www.comp.nus.edu.sg/~cs2030/style/">y</a><a href="https://www.comp.nus.edu.sg/~cs2030/style/">le Guide</a><strong>Problem Description</strong>The Kent Ridge Cruise Centre has just opened and you are required to design a program to decide how many loaders to buy based on a single-day cruise schedule.<strong>Cruises</strong>Each cruise has four attributes:a unique string identifier, e.g. S1234a time of arrival in HHMM format, e.g. 2359 denoting the cruise arriving at 11:59PM on that day the time needed to serve the cruise (in minutes), and the number of loaders needed to serve the cruise.Every cruise must be served by loaders immediately upon arrival.There are two types of cruises:SmallCruise:has an identifier that starts with an uppercase character S; takes a fixed 30 minutes for a loader to fully load; requires only one loader for it to be fully served; BigCruise:has an identifier that starts with an uppercase character B;takes one minute to serve every 50 passengers; requires one loader per every 40 meters in length of the cruise (or part thereof) to fully load<strong>Loaders</strong>Loaders have to be purchased to serve cruises. Each loader comprises two attributes:a unique integer identifierthe cruise that it is currently serving</td>

  </tr>

 </tbody>

</table>

<table width="606">

 <tbody>

  <tr>

   <td width="606">class Cruise {private final String identifier;     private final int arrivalTime;     private final int numOfLoader;     private final int serviceTime; … }</td>

  </tr>

 </tbody>

</table>

A loader will serve a cruise as soon as it arrives, and continues to do so until the service time has elapsed (i.e. it cannot serve a cruise while in the midst of serving another one).

For example, if an incoming cruise arrives at 12PM, requires two loaders, and 60 min for it to be fully served, then, at 12PM, there must be two vacant loaders. These two loaders will serve the cruise from 12PM – 1PM. They can only serve another cruise from 1PM onwards.

<h1>Task</h1>

Your task is to determine the loader allocation schedule using the following steps:

For each cruise, check through the inventory of existing loaders, starting from the loader first purchased, and so on;

The first (or first few) loaders available will be used to serve the cruise;

If there are not enough loaders, purchase new one(s) to serve the cruise.

Take note of the following assumptions:

Input cruises are presented chronologically by arrival time.

There are no duplicate cruises.

All cruises will arrive and be completely served within a single day.

Although this problem can be implemented procedurally, you are to model your solution using an object-oriented approach instead.

This task is divided into several levels. You need to complete ALL levels.

<strong>Level 1</strong>

<h1>Represent a Cruise</h1>

Design an <em>immutable</em> Cruise class to represent a cruise having a unique identifier string, the time of arrival as an integer, the number of loaders required to load the cruise as an integer, and the service time in minutes as an integer.

Note that the time of arrival is in HHMM format. Specifically, 0 or 0000 refers to 00:00 (12AM), 30 or 0030 refers to 00:30 (12:30AM), and 130 or 0130 refers to 01:30 (1:30AM).

Implement a getServiceCompletionTime method, which returns the time the service completes (in number of minutes) since midnight, and a getArrivalTime method, which returns the arrival time (in number of minutes) since midnight.

For example, if the cruise arrives at 12PM (noon time), the arrival time is (12 * 60) = 720; the service completion time is 12:30PM, which is 750 minutes since midnight, i.e. (12 * 60) + 30 = 750.

In addition, implement a getNumOfLoadersRequired method, which returns the number of loaders required to load the cruise.

Tip: the %0Xd format specifier might be of use to you, where the integer will be represented by an X-digit zero-padded number. For instance,

String.format(“%04d”, 20);

would return the string 0020.

$ javac your_java_files

$ jshell your_java_files_in_bottom-up_dependency_order

jshell&gt; new Cruise(“A1234”, 0, 2, 30)

$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0c4d3d3e3f384c3c3c3c3c">[email protected]</a>

jshell&gt; new Cruise(“A2345”, 30, 2, 30)

$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2e6f1c1d1a1b6e1e1e1d1e">[email protected]</a>

jshell&gt; new Cruise(“A3456”, 130, 2, 30)

$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="94d5a7a0a1a2d4a4a5a7a4">[email protected]</a>

jshell&gt; new Cruise(“A3456”, 130, 2, 30).getArrivalTime() $.. ==&gt; 90

<table width="606">

 <tbody>

  <tr>

   <td width="606">jshell&gt; new Cruise(“A3456”, 130, 2, 30).getNumOfLoadersRequired()$.. ==&gt; 2jshell&gt; new Cruise(“A3456”, 130, 5, 30).getNumOfLoadersRequired()$.. ==&gt; 5jshell&gt; new Cruise(“A1234”, 0, 2, 30).getServiceCompletionTime()$.. ==&gt; 30jshell&gt; new Cruise(“A1234”, 0, 2, 45).getServiceCompletionTime()$.. ==&gt; 45jshell&gt; new Cruise(“CS2030”, 1200, 2, 100).getServiceCompletionTime()$.. ==&gt; 820jshell&gt; new Cruise(“D1010”, 2329, 2, 30).getServiceCompletionTime()$.. ==&gt; 1439 jshell&gt; /exit</td>

  </tr>

 </tbody>

</table>

<table width="606">

 <tbody>

  <tr>

   <td width="606">class Loader {private final int identifier;     private final Cruise cruise;… }</td>

  </tr>

 </tbody>

</table>

<table width="606">

 <tbody>

  <tr>

   <td width="606">$ javac your_java_files$ jshell your_java_files_in_bottom-up_dependency_order jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5110606362651161616161">[email protected]</a>jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).getIdentifier()$.. ==&gt; 1jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).getNextAvailableTime()$.. ==&gt; 30jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).canServe(new Cruise(“A2345”, 30, 1, 30))$.. ==&gt; truejshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 30, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4a0b78797e7f0a7a7a797a">[email protected]</a>jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 30, 1, 30)).getNex$.. ==&gt; 60jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).canServe(new Cruise(“A2345”, 10, 1, 30))$.. ==&gt; falsejshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 10, 1, 30))$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0342323130374333333333">[email protected]</a>jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 10, 1, 30)).getNex$.. ==&gt; 30jshell&gt; new Loader(1, new Cruise(“A1234”, 0, 1, 30)).serve(new Cruise(“A2345”, 10, 1, 30)).getIde$.. ==&gt; 1 jshell&gt; /exit</td>

  </tr>

 </tbody>

</table>

<h1>Level 2</h1>

<h1>Use Loaders to serve Cruises</h1>

Design an <em>immutable</em> Loader class to serve a cruise.

Include the following in the Loader class:

a constructor that takes in an integer denoting its unique identifier, as well as the first cruise that it serves. a canServe(Cruise) method that returns true if the loader is available to serve the given cruise, or false otherwise.

a serve(Cruise) method to serve a given cruise. If the loader is available, the method returns the loader serving this cruise; otherwise the existing loader is returned the methods getIdentifier() and getNextAvailableTime() to return the loader’s identifier, and the next available time for service.

<strong>Level 3</strong>

<h1>Represent Small Cruises</h1>

Design the SmallCruise class. The arguments of the constructor are its identifier, and time of arrival. Note that you should not need to change your Loader class if you have implemented it properly.

$ javac your_java_files

<table width="606">

 <tbody>

  <tr>

   <td width="606">$ javac your_java_files$ jshell your_java_files_in_bottom-up_dependency_order jshell&gt; Cruise b = new BigCruise(“B0001”, 0, 70, 3000)jshell&gt; b.getArrivalTime()$.. ==&gt; 0jshell&gt; b.getServiceCompletionTime()$.. ==&gt; 60jshell&gt; b.getNumOfLoadersRequired()$.. ==&gt; 2jshell&gt; new Loader(1, b).serve(b) $.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e6a4d6d6d6d7a6d6d6d6d6">[email protected]</a>jshell&gt; new Loader(1, b).serve(b).getNextAvailableTime()$.. ==&gt; 60jshell&gt; new Loader(2, b)$.. ==&gt; Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5a186a6a6a6b1a6a6a6a6a">[email protected]</a> jshell&gt; new Loader(3, b)$.. ==&gt; Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e9abd9d9d9d8a9d9d9d9d9">[email protected]</a>jshell&gt; new Loader(4, new BigCruise(“B2345”, 0, 30, 1450)).serve(new SmallCruise(“S0000”, 29))$.. ==&gt; Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4a197a7a7a7a0a7a7a7873">[email protected]</a>jshell&gt; new Loader(5, new BigCruise(“B3456”, 0, 75, 1510)).serve(new SmallCruise(“S0001”, 30))$.. ==&gt; Loader 5 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a7e594939291e797979797">[email protected]</a>jshell&gt; /exit</td>

  </tr>

 </tbody>

</table>

<table width="606">

 <tbody>

  <tr>

   <td width="606">$ javac your_java_files$ jshell your_java_files_in_bottom-up_dependency_orderjshell&gt; List&lt;Cruise&gt; cruises = List.of(new SmallCruise(“S1111”, 1300))cruises ==&gt; [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4a197b7b7b7b0a7b797a7a">[email protected]</a>] jshell&gt; serveCruises(cruises)Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="bdee8c8c8c8cfd8c8e8d8d">[email protected]</a> jshell&gt; cruises = List.of(new BigCruise(“B1111”, 1300, 80, 3000),…&gt;     new SmallCruise(“S1111”, 1359),…&gt;     new SmallCruise(“S1112”, 1400),     …&gt;     new SmallCruise(“S1113”, 1429))cruises ==&gt; [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e0a2d1d1d1d1a0d1d3d0d0">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="eebddfdfdfdfaedfdddbd7">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c99af8f8f8fb89f8fdf9f9">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e6b5d7d7d7d5a6d7d2d4df">[email protected]</a>] jshell&gt; serveCruises(cruises) Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cb89fafafafa8bfaf8fbfb">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c082f1f1f1f180f1f3f0f0">[email protected]</a></td>

  </tr>

 </tbody>

</table>

$ jshell your_java_files_in_bottom-up_dependency_order jshell&gt; new SmallCruise(“S0001”, 0).getArrivalTime() $.. ==&gt; 0

jshell&gt; new SmallCruise(“S0001”, 0).getServiceCompletionTime()

$.. ==&gt; 30

jshell&gt; new SmallCruise(“S0001”, 0).getNumOfLoadersRequired()

$.. ==&gt; 1

jshell&gt; (Cruise) new SmallCruise(“S0123”, 1220)

$.. ==&gt; <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b2e182838081f283808082">[email protected]</a>

jshell&gt; new Loader(1, new SmallCruise(“S1245”, 2330))

$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b8eb898a8c8df88a8b8b88">[email protected]</a>

jshell&gt; new Loader(1, new SmallCruise(“S1245”, 2330)).canServe(new SmallCruise(“S2345”, 2359))

$.. ==&gt; false

jshell&gt; new Loader(1, new SmallCruise(“S1245”, 2330)).serve(new SmallCruise(“S2345”, 2359))

$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1241232026275220212122">[email protected]</a>

jshell&gt; new Loader(1, new SmallCruise(“S2030”, 0))

$.. ==&gt; Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7f2c4d4f4c4f3f4f4f4f4f">[email protected]</a> jshell&gt; /exit

<strong>Level 4</strong>

<h1>Represent Big Cruises</h1>

Design the BigCruise class. The arguments of the constructor are its identifier, time of arrival, the length of the cruise, and number of passengers, in that order.

<strong>Level 5</strong>

<h1>Output the loader allocation schedule</h1>

Write a method void serveCruises(List&lt;Cruise&gt; cruises) that takes in a list of cruises, and outputs the allocation schedule of the loaders required to service all the cruises. Save the method in the file level5.jsh.




<table width="681">

 <tbody>

  <tr>

   <td rowspan="2" width="37"> </td>

   <td width="607">Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="8ad9bbbbbbbbcabbb9bfb3">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2d7e1c1c1c1f6d1c191d1d">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5f0c6e6e6e6c1f6e6b6d66">[email protected]</a> jshell&gt; cruises = List.of(new SmallCruise(“S1111”, 900),…&gt;     new BigCruise(“B1112”, 901, 100, 1),…&gt;     new BigCruise(“B1113”, 902, 20, 4500),…&gt;     new SmallCruise(“S2030”, 1031),…&gt;     new BigCruise(“B0001”, 1100, 30, 1500),…&gt;     new SmallCruise(“S0001”, 1130))cruises ==&gt; [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="92c1a3a3a3a3d2a2aba2a2">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="ecaedddddddeacdcd5dcdd">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b1f380808082f181888183">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="9dceafadaeadddacadaeac">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="bcfe8c8c8c8dfc8d8d8c8c">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="bdee8d8d8d8cfd8c8c8e8d">[email protected]</a>] jshell&gt; serveCruises(cruises) Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cc9ffdfdfdfd8cfcf5fcfc">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5012616161621060696061">[email protected]</a>Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0e4c3f3f3f3c4e3e373e3f">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="16542727272456262f2627">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="53116262626013636a6361">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f5a6c7c5c6c5b5c4c5c6c4">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7032404040413041414040">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3c6f0c0c0c0d7c0d0d0f0c">[email protected]</a></td>

   <td rowspan="2" width="37"> </td>

  </tr>

  <tr>

   <td width="607"><strong>Level 6</strong><strong>Recycled Loaders</strong>The objective of this level is to determine whether your current implementation can be easily extended with minimal modification to the clientThe Cruise Centre has just introduced a new eco-friendly policy in an effort to go green. Their policy states that every third loader that is purchased must be made of recycled materials (referred to as recycled loaders). These recycled loaders will go through a 60-minute long maintenance after every service. It is unable to serve any cruise during this period.For example, if a recycled loader serves a SmallCruise that arrives at 12:30PM, then the next time the loader can serve another Cruise is 2PM (30min + 60min after 12:30PM).By modifying level5.jsh, incorporate the above with minimal modifications to the file level6.jsh.

    <table width="606">

     <tbody>

      <tr>

       <td width="606">$ javac your_java_files$ jshell your_java_files_in_bottom-up_dependency_orderjshell&gt; List&lt;Cruise&gt; cruises = List.of(new BigCruise(“B1111”, 0, 60, 1500),…&gt;     new SmallCruise(“S1112”, 0),…&gt;     new BigCruise(“B1113”, 30, 100, 1500),…&gt;     new BigCruise(“B1114”, 100, 100, 1500),…&gt;     new BigCruise(“B1115”, 130, 100, 1500),    …&gt;     new BigCruise(“B1116”, 200, 100, 1500))cruises ==&gt; [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3f7d0e0e0e0e7f0f0f0f0f">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1744262626255727272727">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="86c4b7b7b7b5c6b6b6b5b6">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="fdbfccccccc9bdcdcccdcd">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f8bac9c9c9cdb8c8c9cbc8">[email protected]</a>, <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7436454545423444464444">[email protected]</a>] jshell&gt; serveCruises(cruises) Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c684f7f7f7f786f6f6f6f6">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="de9cefefefef9eeeeeeeee">[email protected]</a>Recycled Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3f6c0e0e0e0d7f0f0f0f0f">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="86c4b7b7b7b5c6b6b6b5b6">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="90d2a1a1a1a3d0a0a0a3a0">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f7b5c6c6c6c4b7c7c7c4c7">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="98daa9a9a9acd8a8a9a8a8">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0f4d3e3e3e3b4f3f3e3f3f">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="81c3b0b0b0b5c1b1b0b1b1">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c587f4f4f4f085f5f4f6f5">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6e2c5f5f5f5b2e5e5f5d5e">[email protected]</a>Recycled Loader 3 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1456252525215424252724">[email protected]</a>Loader 1 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b1f380808087f181838181">[email protected]</a>Loader 2 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1f5d2e2e2e295f2f2d2f2f">[email protected]</a>Loader 4 serving <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b1f380808087f181838181">[email protected]</a></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

  <tr>

   <td colspan="3" width="681"></td>

  </tr>

 </tbody>

</table>


