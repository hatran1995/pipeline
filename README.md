# pipeline

# Partie 1
1. Quelles sont les ressources Terraform manquantes ?
  il manque le datastream pour la réception de flux de données et la destination du pipeline
2. On va créer
 - un fichier datastream :  kinesis_datastream.tf
 Code : 
                 resource "aws_kinesis_stream" "kinesis_input_stream" {
                  name        = "input-stream"
                  shard_count = 1
                  # The defaut retention period is 24h
                  retention_period = 24

                  shard_level_metrics = [
                    "IncomingBytes",
                    "OutgoingBytes",
                  ]

                  tags = {
                    Environment = "test"
                  }
                }

 - un fichier de destination S3 bucket : destination_s3_bucket.tf
  Code :  # Create a S3 bucket - destination of the data pipeline
        resource "aws_s3_bucket" "analytics_destination" {
          bucket = "analytics-destination-wxcft6ha261195"
          acl    = "private"

          tags = {
            Name        = "S3 bucket"
            Environment = "test"
          }
        }
  3.
  4.
  5. La partie "user_data" dans le fichier "ec2.tf" est pour but d'installer tous les librairies et framworks nécessaires pour faire marcher l'application de logs.
  6. L'agent Kinesis sur la VM EC2 permet de collecter et d'envoyer facilement des données à Kinesis Data Streams.
  7.
 # Partie #2
 Quelle est l’architecture de données de ce « data pipeline » ?
1. Ingestion - récupérer et importer toutes les données brutes depuis les sources de données
 Outil utilisé : AWS Kinesis Data Stream, Kinesis Delivery Stream, 
3. Stockage - stocker les données
Outil utilisé : AWS S3 Bucket
5. Transformation - traiter les données 
Outil utilisé : AWS Kinesis Data Analytics
7. Exposition - accès au résultat de transformation des données
Outil utilisé : Requetes SQL
 
