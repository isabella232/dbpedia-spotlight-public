# DBpedia Spotlight

##### Building and running.

```
$ mvn clean package
$ java -cp "rest/target/rest-0.7.jar" org.dbpedia.spotlight.web.rest.Server en http://localhost:2222/rest
```

#### Models and data

Models and raw data for most languages are available [here](http://spotlight.sztaki.hu/downloads/).

#### Building your own models

The model-quickstarter repo [here](https://github.com/jodaiber/model-quickstarter) can be used to build models.
This essentially links to the `bin/index_db.sh` script.

#### Code structure

##### REST Module

The class responsible for running the web-server `org.dbpedia.spotlight.web.rest.Server`
The annotate REST resource is `org.dbpedia.spotlight.web.rest.resources.Annotate`

##### Index Module
The class responsible for building the Spotlight Model is `org.dbpedia.spotlight.db.CreateSpotlightModel`

##### Core Module
The web-server loads the models using this module, it uses the `org.dbpedia.spotlight.db.SpotlightModels`
This requires the input models folder which is responsible for loading the
- Tokenizer
- Stemmer
- Spotters
- Disambiguators

**Using OpenNLP for tokenization and spotting**
Input model folder must contain an `opennlp` folder, with files listed below
- files `{pos-maxent,sent,token}.bin` to load the `org.dbpedia.spotlight.db.tokenize.OpenNLPTokenizer`
- files `[ner-{location,person,organization}.bin , chunker.bin]` to load the `org.dbpedia.spotlight.db.OpenNLPSpotter`

**Defaults**
The default values used are
- Tokenizer: reads the `model.properties` in the input models and uses the locale to create a `org.dbpedia.spotlight.db.tokenize.LanguageIndependentTokenizer`.
- Stemmer: The Stemmer is defaulted to `SnowballStemmer` for English, its non-configurable.
- Spotters: The default spotter used is `org.dbpedia.spotlight.db.FSASpotter`, this is loaded of a `fsa_dict.mem` file.
- Candidate search: This is based on the
`{model/{sf,res,cadmap}.mem}` files which are used in the `org.dbpedia.spotlight.db.DBCandidateSearcher` class
- Disambiguators: The default disambiguator implementation used is `org.dbpedia.spotlight.db.DBTwoStepDisambiguator`
it relies on the searcher models (described above) + `model/{tokens,context}.mem` to construct `DBpediaResourceOccurence` and similarity scores.
