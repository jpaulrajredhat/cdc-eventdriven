
curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" 127.0.0.1:8083/connectors/ --data "@debezium.json"

curl -s http://localhost:8083/connector-plugins | jq

docker run --tty \
--network rmoff_kafka \
confluentinc/cp-kafkacat \
kafkacat -b kafka:9092 -C \
-s key=s -s value=avro \
-r http://schema-registry:8085 \
-t postgres.public.ingredients


docker run --tty \
--network rmoff_kafka \
confluentinc/cp-kafkacat \
kafkacat -b kafka:9092 -C \
-s key=s -s value=avro \
-r http://schema-registry:8085 \
-t postgres.public.recipes

INSERT INTO recipes
 (recipe_id, recipe_name) 
VALUES
 (4,'Tacos'),
 (5,'Tomato Soup'),
 (6,'Grilled Cheese');

INSERT INTO ingredients
    (ingredient_id, ingredient_name, ingredient_price)
VALUES 
    (1, 'Beef', 5),
    (2, 'Lettuce', 1),
    (3, 'Tomatoes', 2);
