# java-tensorflow-music

Music classification, music search, music recommender and music encoder implemented in Tensorflow and Java

The trained models were obtained from the [Keras audio deep learning project](https://github.com/chen0040/java-audio-embedding) 

# Install

Add the following dependency to your POM file:

```xml
<dependency>
  <groupId>com.github.chen0040</groupId>
  <artifactId>java-tensorflow-music</artifactId>
  <version>1.0.1</version>
</dependency>
```

# Usage

### Run audio classifier in Java
 
The [sample codes](java_audio_classifier/src/main/java/com/github/chen0040/tensorflow/classifiers/demo/Cifar10AudioClassifierDemo.java) 
below shows how to use the cifar audio classifier to predict the genres of music:

```java
import com.github.chen0040.tensorflow.classifiers.models.cifar10.Cifar10AudioClassifier;
import com.github.chen0040.tensorflow.classifiers.utils.ResourceUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Demo {
    public static void main(String[] args) {
        
        Cifar10AudioClassifier classifier = new Cifar10AudioClassifier();
        classifier.load_model();
        
        List<String> paths = getAudioFiles();
        
        Collections.shuffle(paths);
        
        for (String path : paths) {
            System.out.println("Predicting " + path + " ...");
            File f = new File(path);
            String label = classifier.predict_audio(f);
        
            System.out.println("Predicted: " + label);
        }
    }
}
```  

 
The [sample codes](java_audio_classifier/src/main/java/com/github/chen0040/tensorflow/classifiers/demo/ResNetV2AudioClassifierDemo.java) 
below shows how to use the resnet v2 audio classifier to predict the genres of music:

```java
import com.github.chen0040.tensorflow.classifiers.resnet_v2.ResNetV2AudioClassifier;
import com.github.chen0040.tensorflow.classifiers.utils.ResourceUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Demo {
    public static void main(String[] args) {
        
        ResNetV2AudioClassifier classifier = new ResNetV2AudioClassifier();
        classifier.load_model();
        
        List<String> paths = getAudioFiles();
        
        Collections.shuffle(paths);
        
        for (String path : paths) {
            System.out.println("Predicting " + path + " ...");
            File f = new File(path);
            String label = classifier.predict_audio(f);
        
            System.out.println("Predicted: " + label);
        }
    }
}
```  

### Extract features from audio in Java

The [sample codes](java_audio_classifier/src/main/java/com/github/chen0040/tensorflow/classifiers/demo/Cifar10AudioEncoderDemo.java) 
below shows how to use the cifar audio classifier to encode an audio file into an float array:

```java
import com.github.chen0040.tensorflow.classifiers.models.cifar10.Cifar10AudioClassifier;
import com.github.chen0040.tensorflow.classifiers.utils.ResourceUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Demo {
    public static void main(String[] args){
        
        Cifar10AudioClassifier classifier = new Cifar10AudioClassifier();
        classifier.load_model();
        
        List<String> paths = getAudioFiles();
        
        Collections.shuffle(paths);
        
        for (String path : paths) {
            System.out.println("Encoding " + path + " ...");
            File f = new File(path);
            float[] encoded_audio = classifier.encode_audio(f);
        
            System.out.println("Encoded: " + Arrays.toString(encoded_audio));
        }
    }
}
```  

 
The [sample codes](java_audio_classifier/src/main/java/com/github/chen0040/tensorflow/classifiers/demo/ResNetV2AudioEncoderDemo.java) 
below shows how to the resnet v2 audio classifier to encode an audio file into an float array:

```java
import com.github.chen0040.tensorflow.classifiers.resnet_v2.ResNetV2AudioClassifier;
import com.github.chen0040.tensorflow.classifiers.utils.ResourceUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Demo {
    public static void main(String[] args) {
        
        ResNetV2AudioClassifier classifier = new ResNetV2AudioClassifier();
        classifier.load_model();
        
        List<String> paths = getAudioFiles();
        
        Collections.shuffle(paths);
        
        for (String path : paths) {
            System.out.println("Encoding " + path + " ...");
            File f = new File(path);
            float[] encoded_audio = classifier.encode_audio(f);
        
            System.out.println("Encoded: " + Arrays.toString(encoded_audio));
        }
    }
}
```  

### Audio Search Engine

The [sample codes](java_audio_search/src/main/java/com/github/chen0040/tensorflow/search/AudioSearchEngineDemo.java) 
below shows how to index and search for audio file using the [AudioSearchEngine](java_audio_search/src/main/java/com/github/chen0040/tensorflow/search/models/AudioSearchEngine.java) class:

```java
import com.github.chen0040.tensorflow.search.models.AudioSearchEngine;
import com.github.chen0040.tensorflow.search.models.AudioSearchEntry;

import java.io.File;
import java.util.List;

public class Demo {
    public static void main(String[] args){
        AudioSearchEngine searchEngine = new AudioSearchEngine();
        if(!searchEngine.loadIndexDbIfExists()) {
            searchEngine.indexAll(FileUtils.getAudioFiles());
            searchEngine.saveIndexDb();
        }
        
        int pageIndex = 0;
        int pageSize = 20;
        boolean skipPerfectMatch = true;
        File f = new File("mp3_samples/example.mp3");
        System.out.println("querying similar music to " + f.getName());
        List<AudioSearchEntry> result = searchEngine.query(f, pageIndex, pageSize, skipPerfectMatch);
        for(int i=0; i < result.size(); ++i){
            System.out.println("# " + i + ": " + result.get(i).getPath() + " (distSq: " + result.get(i).getDistance() + ")");
        }
    }
}
```  

### Music Recommend-er

The [sample codes](java_audio_recommender/src/main/java/com/github/chen0040/tensorflow/search/KnnAudioRecommenderDemo.java) 
below shows how to recommend musics based on user's music history using the [KnnAudioRecommender](java_audio_recommender/src/main/java/com/github/chen0040/tensorflow/search/models/KnnAudioRecommender.java) class:

```java
import com.github.chen0040.tensorflow.classifiers.utils.FileUtils;
import com.github.chen0040.tensorflow.recommenders.models.AudioUserHistory;
import com.github.chen0040.tensorflow.recommenders.models.KnnAudioRecommender;
import com.github.chen0040.tensorflow.search.models.AudioSearchEntry;

import java.io.File;
import java.util.Collections;
import java.util.List;

public class Demo {
    public static void main(String[] args){
        // create fake listening history of songs
        AudioUserHistory userHistory = new AudioUserHistory();
        
        List<String> audioFiles = FileUtils.getAudioFilePaths();
        Collections.shuffle(audioFiles);
        
        for(int i=0; i < 40; ++i){
            String filePath = audioFiles.get(i);
            userHistory.logAudio(filePath);
            try {
                Thread.sleep(100L);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        
        KnnAudioRecommender recommender = new KnnAudioRecommender();
        if(!recommender.loadIndexDbIfExists()) {
            recommender.indexAll(new File("music_samples").listFiles(a -> a.getAbsolutePath().toLowerCase().endsWith(".au")));
            recommender.saveIndexDb();
        }
        
        System.out.println(userHistory.head(10));
        
        int k = 10;
        List<AudioSearchEntry> result = recommender.recommends(userHistory.getHistory(), k);
        
        for(int i=0; i < result.size(); ++i){
            AudioSearchEntry entry = result.get(i);
            System.out.println("Search Result #" + (i+1) + ": " + entry.getPath());
        }
    }
}

```












