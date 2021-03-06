## Configurable converters

For some types, PureConfig cannot automatically derive a converter because there are multiple ways to convert a configuration value to them. For instance, for [`LocalDate`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html) PureConfig cannot derive a converter because there are multiple [`DateTimeFormatter`](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)s that can be used to convert a String into a `LocalDate`. Examples of different formats are `yyyy-mm-dd`, e.g. `"2016-01-01"`, and `yyyymmdd`, e.g. `"20160101"`.

For those types, PureConfig provides a way to create converters from the necessary parameters. These methods can be found under the package `pureconfig.configurable`. Once the output of a `pureconfig.configurable` method for a certain type is in scope, PureConfig can start using that configured converter.

Define a case class to hold your configuration, and create a configurable converter:

```scala
import com.typesafe.config.ConfigFactory.parseString
import java.time.LocalDate
import java.time.format.DateTimeFormatter
import pureconfig.configurable._
import pureconfig.loadConfig

case class Conf(date: LocalDate)

implicit val localDateInstance = localDateConfigConvert(DateTimeFormatter.ISO_DATE)

val conf = parseString(s"""{ date: "2011-12-03" }""")
```
Then load the configuration:

```scala
loadConfig[Conf](conf)
// res3: Either[pureconfig.error.ConfigReaderFailures,Conf] = Right(Conf(2011-12-03))
```
