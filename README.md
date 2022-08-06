# LargeFileTask
Unable to onboard a large file task

Problem :- A large file(67 mb file) was unable to upload through 3 microservices, so we need to process this
Analysis :- there was one method which dont handle this kind on bytes(22 cr bytes) when i print the length of string
So i need to somehow process this large file i.e onboarding this file
As this ToscaExtensionYamlUtil().yamlToObject<ToscaServiceModel>(YamlString,ToscaServiceModel::class.java) method does not take that much amount of length of string so we seperate the ArtifactFile String and YamlString.
So only YamlString is passed to this method, so ToscaServiceModel will be created without ArtifactFileSection.
Then after that we can manipulate that string and convert into a HashMap and then we can stitch to the ToscaServiceModel which will have the ArtifactFileSection, thats what we have used for solving this problem.

As it was written in kotlin, so i need to write code in kotlin, so somehow i have written the code

Solution :- I need to do String manipulation
I need remove the large string from the given and pass it to the method for proess and take the response and the stick it to the original string for onboarding
Code :- 
        
        val yamlutil = ToscaExtensionYamlUtil()
        val toscaServiceModel: ToscaServiceModel
        val map: HashMap<String, ByteArray> = HashMap<String, ByteArray>()
      
        var artifactFileString: String = translation.result.substring(0, translation.result.indexOf("serviceTemplates"))
        var YamlString: String = translation.result.substring(translation.result.indexOf("serviceTemplates"))
        toscaServiceModel=yamlutil.yamlToObject<ToscaServiceModel>(YamlString,ToscaServiceModel::class.java)
        YamlString=""

        artifactFileString=artifactFileString.replace("artifactFiles:\n","").replace("files:\n","")
        while (artifactFileString.length>1){
            val largeFileName: String = artifactFileString.substring(0, artifactFileString.indexOf(":", 0)).trim()
            val start: Int = artifactFileString.indexOf(largeFileName)
            val firstN: Int = artifactFileString.indexOf("\n", start)
            var largeArtifactFileString = ""
            var mapString = ""
            if (artifactFileString.indexOf("\n", firstN + 1) != -1) {
                largeArtifactFileString = artifactFileString.substring(start, artifactFileString.indexOf("\n", firstN + 1))
                artifactFileString = artifactFileString.replace(largeArtifactFileString, "").trim()
                mapString = largeArtifactFileString.substring(largeArtifactFileString.indexOf("\n")).replace("\n", "").trim()
            }else{
                largeArtifactFileString = artifactFileString.substring(start, artifactFileString.length)
                mapString = largeArtifactFileString.substring(largeArtifactFileString.indexOf("\n")).replace("\n", "").trim()
                artifactFileString = artifactFileString.replace(largeArtifactFileString, "").trim()
            }
            map.put(largeFileName,mapString.toByteArray())
        }
        artifactFileString=""
        val files=FileContentHandler()
        files.putAll(map)
        toscaServiceModel.setArtifactFiles(files)


Limitation :- Number of resources and heap space also needs to be increased for this task. 
