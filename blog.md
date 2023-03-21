### Duplicates
- No duplicate entries found

### Missing Values
- very first time periods contain higher proportion of missing values which may bias statistical results. From 2017 onwards is better ideas.
- ies_siglas_instit in the table ASIGNACIONES is redundant or rather unnecessary information which prompts to be revomed.
- provincia_reside, canton_reside, parroquia_reside in the table INSCRIPCIONES contain fewer mising values than their counterparts in the table ASIGNACIONES. Thus, I'll proceed to remove those three from ASIGNACIONES. 
- Both tables INSCRIPCIONES and ASIGNACIONES contain the usu_nacionalidad columns. One of them contain more missing values than the other. Also, values may not fully correspond each other. I need to get maximum amount of information. How to solve this recurrent problem? 

### Non-Numeric Types
- INSCRIPCIONES::canton_reside : 221 true districts contrasts to 225 in-data districts. May pass through for now
- INSCRIPCIONES::parroquia_reside :  1.499 true parishes contrast to 1172 in-data parishes. May pass through for now
- How to rename columns with a lambda function? You specify that it is the columns you want to change. Then, you write the lambda function now understanding that you're accessing each of the column names. `df_.rename(columns=lambda c: c.replace(' ', '_').lower())`
- In the table ASIGNACIONES, I removed the column usu_nacionalidad
- ASIGNACIONES::nombre_ies : high cardinality 386 categories possible ; some values are mistypes but the effort is not worth it on a first round.
- ASIGNACIONES::provincia_ies : caaar & canar are the same thing
- ASIGNACIONES::canton_ies 
- ASIGNACIONES::parroquia_ies high cardinality 131 categories possible; 1.499 true parishes contrast to 131 in-data parishes. Although expected as there are far fewer universities than individuals.
- ASIGNACIONES::carrera
- ASIGNACIONES::canton_ies medium to high cardinality 87 categories possible;
- INSCRIPCIONES&ASIGNACIONES&POSTULACIONES: parroquia column could be potentially divided into three columns designation the city, whether or not it is `cabecera cantonal`, whether or not it is `capital de provincia`, whether or not it is `capital del ecuador`.
    

### Tweak Function Rally
- Each of the tweak function must return data ready to use in production. No single additional operation should be needed to remove NaNs, duplicates, validate data.


### Off-grid ideas
In my data analysis work is common to work with many tables that should all contain the same variables but correspond to different time periods. What usually occurs when trying to concatenate is that the number of columns is different and columns name are sometimes different although they mean represent the same variable. How do I solve this recurrent problem? The solution must go along the lines of collecting precise information about each variable and presenting it in manner that allow for easy comparison.
Idea 1: Create a table where one axis consists of the column names of the first dataframe and the another consists of the column names of the second. This solution may prove useful when working with two datasets.

I mostly didn't care about the high cardinality variables. I've really only replaced values with low cardinality variables. To be honest, updating/transforming objects data type columns has consisted of updating data type and replacing values.

### TO-DO:
@generate dictionary to replace the wrong values (`azogues, cabecera cantonal`, `la aurora (satalite)`, `iaaquito`) in the parroquia_ies column for asignaciones
@construct dictionaries for cabeceras can
@generate a column indicating which parishes are "cabecera cantonal" and/or "capital provincial" 
@when reading the data from the gcs bucket, read only the selected columns for the end results produced by the tweak function. Make redundant the .drop(columns) step
