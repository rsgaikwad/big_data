Code explanation:
1. Creating a dataset “hello world”
2. Defining a function ‘upper’ which converts a string into upper case.
3. We now import the ‘udf’ package into Spark.
4. Defining our UDF, ‘upperUDF’ and importing our function ‘upper’.
5. Displaying the results of our User Defined Function in a new column ‘upper’.



val dataset = Seq((0, "hello"),(1, "world")).toDF("id","text")
val upper: String => String =_.toUpperCase
import org.apache.spark.sql.functions.udf
val upperUDF = udf(upper)
dataset.withColumn("upper", upperUDF('text)).show


Output :
scala> val dataset = Seq((0, "hello"),(1, "world")).toDF("id","text")
dataset: org.apache.spark.sql.DataFrame = [id: int, text: string]

scala> val upper: String => String =_.toUpperCase
upper: String => String = <function1>

scala> import org.apache.spark.sql.functions.udf
import org.apache.spark.sql.functions.udf

scala> val upperUDF = udf(upper)
upperUDF: org.apache.spark.sql.UserDefinedFunction = UserDefinedFunction(<function1>,StringType,List(StringType))

scala> dataset.withColumn("upper", upperUDF('text)).show
+---+-----+-----+
| id| text|upper|
+---+-----+-----+
|  0|hello|HELLO|
|  1|world|WORLD|
+---+-----+-----+








