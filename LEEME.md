<p align="center"> 
	<img alt="JPlag logo" src="core/src/main/resources/de/jplag/logo-dark.png" width="350">
</p>

# JPlag - Scripting in bash Wsl andlinux for Detecting Source Code Plagiarism with .zip commpressed Moodle exporting Files
[![CI Build](https://github.com/jplag/jplag/actions/workflows/build-maven.yml/badge.svg)](https://github.com/jplag/jplag/actions/workflows/build-maven.yml)
[![Latest Release](https://img.shields.io/github/release/jplag/jplag.svg)](https://github.com/jplag/jplag/releases/latest)
[![Maven Central](https://img.shields.io/maven-metadata/v.svg?metadataUrl=https%3A%2F%2Frepo1.maven.org%2Fmaven2%2Fde%2Fjplag%2Fjplag%2Fmaven-metadata.xml&label=maven-central&color=00ff00)](https://central.sonatype.com/artifact/de.jplag/jplag)
[![License](https://img.shields.io/github/license/jplag/jplag.svg)](https://github.com/jplag/jplag/blob/main/LICENSE)
[![GitHub commit activity](https://img.shields.io/github/commit-activity/y/jplag/JPlag)](https://github.com/jplag/JPlag/pulse)
[![SonarCloud Coverage](https://sonarcloud.io/api/project_badges/measure?project=jplag_JPlag&metric=coverage)](https://sonarcloud.io/component_measures?metric=Coverage&view=list&id=jplag_JPlag)
[![Java Version](https://img.shields.io/badge/java-SE%2025-yellowgreen)](#download-and-installation)
'''
#!/bin/bash
# 1 EXTRAE DE LA CARPETA DE EJECUCION .ZIP QUE DESCOMPRIMIR EN LA CARPETA PARÁMETRO
#       PARÁMETRO $1 el nombre del directorio donde extrae los .zip

# 1.-- Descomprima el contenido de ZIP del directorio de ejecución
#for file in $(ls *.zip)
#for file in $(find . -iname "*.zip")
for file in *.zip
do
if [ -d "$file" ]
 then
        echo directorio: "$file"
 else
        if [ -f "$file" ]
         then
                echo archivo: "$file"

                # DESCOMPRIME A CARPETA V4
                dir=$(echo "$file" | awk -F/ '{ print $2"" }' | tr ' (),:-' '_' )"$1"
                fichero=$( echo "$file" | sed 's/ /\\ /g' )
                echo archivo zip: "$fichero" y directorio: $dir

                # Si existen ficheros de extensión .zip
                if [ "$file" != "*.zip" ] ; then
                        #$(gzip $file ; tar -xvzf $file ; rm $file )
                        salida=$( unzip -O DOS "$file" -d $dir ; rm "$file" )
                fi
       fi
 fi
done


# 2 REVISA EL CONTEIDO DE LAS CARPETAS BUSCANDO .ZIP QUE DESCOMPRIMIR

# 2.1.- descomprima 1er nivel del contenido de los ZIP de TODOS los directorios creados
for file in ./*/*.zip
do
        dir=$( echo "$file" | awk -F.zip '{ print $1"" }' | tr ' (),:-' '_' )"_$1"
        echo archivo zip: "$file" y directorio: $dir
        # Si existen ficheros de extensión .zip
        if [ "$file" != "./*/*.zip" ] ; then
        #       $(gzip $file ; tar -xvzf $file ; rm $file )
                salida=$( unzip -O DOS "$file" -d $dir ; rm "$file" )
        fi
done

# 2.2.- descomprima 2º nivel del contenido de los ZIP de TODOS los directorios creados
for file in ./*/*/*.zip
do
        dir=$( echo "$file" | awk -F.zip '{ print $1"" }' )"_$1"
        echo archivo zip: "$file" y directorio: $dir
        # Si existen ficheros de extensión .zip
        if [ "$file" != "./*/*/*.zip" ] ; then
        #       $(gzip $file ; tar -xvzf $file ; rm $file )
                salida=$( unzip -O DOS "$file" -d $dir ; rm "$file" )
        fi
done


# 2.3.- descomprima 3er nivel del contenido de los ZIP de TODOS los directorios creados
#for file in $(find . -iname "*.zip")
for file in ./*/*/*/*.zip
do
        dir=$( echo "$file" | awk -F.zip '{ print $1"" }')"_$1"
        echo archivo zip: "$file" y directorio: $dir
        # Si existen ficheros de extensión .zip
        if [ "$file" != "./*/*/*/*.zip" ] ; then
        #       $(gzip $file ; tar -xvzf $file ; rm $file )
                salida=$( unzip -O DOS "$file" -d $dir ; rm "$file" )
        fi
done

# 2.4.- descomprima 4º nivel del contenido de los ZIP de TODOS los directorios creados
#for file in $(find . -iname "*.zip")
for file in ./*/*/*/*/*.zip
do
        dir=$( echo "$file" | awk -F.zip '{ print $1"" }' )"_$1"
        echo archivo zip: "$file" y directorio: $dir
        if [ "$file" != "./*/*/*/*/*.zip" ] ; then
        #       $(gzip $file ; tar -xvzf $file ; rm $file )
                salida=$( unzip -O DOS "$file" -d $dir ; rm "$file" )
        fi
done


# 3 REEMPLAZA EL FICHERO "ComputerSystemBase.c*" DE TODOS LOS PROYECTOS
# CON EL ERROR LÉXICO EN LINEA 136 Carácter 25 donde "\%c" -> "\n%c" detectado
# presente en el código desde la Versión V0 en "ComputerSystem.c" y desde la versión
# V1 en la parte de Base del código que con la corrección pertinente se sustituye
# con el fichero conocido por "./ComputerSystemBase.c" del directorio raíz con
# la errata corregida.
# 3.1.- Copia a la fuerza el contenido de C del fichero
for file in $(find . -iname "ComputerSystemBase.c")
#for file in *.zip
do
        if [ "$file" != "./ComputerSystemBase.c" ] ; then
                echo "$file"
                echo cp -f ./ComputerSystemBase.c* "$file"
                salida=$( cp -f ./ComputerSystemBase.c* "$file")
        fi
done
'''
JPlag finds pairwise similarities among a set of multiple programs. It can reliably detect software plagiarism and collusion in software development, even when obfuscated. All similarities are calculated locally; no source code or plagiarism results are ever uploaded online. JPlag supports a large number of languages.

* Script Exec Command
   _./scriptJPlag.sh V1_

* ** jPlag WIN EXEC COMMAND**:
  _"java.exe" --enable-native-access=ALL-UNNAMED -jar jplag-6.3.0-jar-with-dependencies.jar alumnCode
V4AllEspEng -bc baseCodeV4All -l c --overwrite -r results_baseCodeV4AllEspEngScript > salidaV4AllEspEng.txt_
  * Salida
    _Loading Submissions 100% [=======================] 177/177 (0:00:04 / 0:00:00)
    Parsing Submissions 100% [=======================] 177/177 (0:00:34 / 0:00:00)
    Preparing Submissions 100% [=====================] 173/173 (0:00:00 / 0:00:00)
    Comparing Submission Pairs 100% [============] 14878/14878 (0:00:04 / 0:00:00)
    Finding Clusters  <<+>         > 0:00:00
			Finding Clusters  < <+>        > 0:00:00
			Finding Clusters  <  <+>       > 0:00:00
			Finding Clusters  <   <+>      > 0:00:00
			Finding Clusters  <    <+>     > 0:00:00
			Finding Clusters  <     <+>    > 0:00:01
			Finding Clusters  <      <+>   > 0:00:01
			Finding Clusters  <       <+>  > 0:00:01
			Finding Clusters  <        <+> > 0:00:01
			Finding Clusters  <         <+>> 0:00:01
			Finding Clusters  <        <+> > 0:00:02
			Finding Clusters  <       <+>  > 0:00:02
			Finding Clusters  <      <+>   > 0:00:02
			Finding Clusters  <     <+>    > 0:00:02
			Finding Clusters  <    <+>     > 0:00:02
			Finding Clusters  <   <+>      > 0:00:03
			Finding Clusters  <  <+>       > 0:00:03
			Finding Clusters  < <+>        > 0:00:03
			Finding Clusters  <<+>         > 0:00:03
			Finding Clusters  < <+>        > 0:00:03
			Finding Clusters  <  <+>       > 0:00:04
			Finding Clusters  <   <+>      > 0:00:04
			Finding Clusters : complete_
*  ** jPlag WIN VIEW COMMAND**:
  "java.exe" --enable-native-access=ALL-UNNAMED -jar jplag-6.3.0-jar-with-dependencies.jar --port=2999 results_UTF-8.jplag

* 📖 [JPlag Wiki](https://github.com/jplag/JPlag/wiki)
* 📈 [JPlag Demo](https://jplag.github.io/Demo/) 
* 🏛️ [JPlag on Helmholtz RSD](https://helmholtz.software/software/jplag)
* 🤩 [Give us Feedback in a **short (<5 min) survey**](https://docs.google.com/forms/d/e/1FAIpQLSckqUlXhIlJ-H2jtu2VmGf_mJt4hcnHXaDlwhpUL3XG1I8UYw/viewform?usp=sf_link)


## Supported Languages

All supported languages and their supported versions are listed below.
