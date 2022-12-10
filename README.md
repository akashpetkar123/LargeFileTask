Company - Amdocs, Pune
# LargeFileTask
Unable to onboard a large file task

Problem :- A large file(67 mb file) was unable to upload through 3 microservices, so we need to process this
Analysis :- there was one method which dont handle this kind on bytes(22 cr bytes) when i print the length of string
So i need to somehow process this large file i.e onboarding this file
As there is one  method does not take that much amount of length of string so we seperate the ArtifactFile String and YamlString.
So only YamlString is passed to this method, so ToscaServiceModel will be created without ArtifactFileSection.
Then after that we can manipulate that string and convert into a HashMap and then we can stitch to the ToscaServiceModel which will have the ArtifactFileSection, thats what we have used for solving this problem.

As it was written in kotlin, so i need to write code in kotlin, so somehow i have written the code

Solution :- I need to do String manipulation
I need remove the large string from the given and pass it to the method for proess and take the response and the stick it to the original string for onboarding

Solution Attached.

Limitation :- Number of resources and heap space also needs to be increased for this task. 
