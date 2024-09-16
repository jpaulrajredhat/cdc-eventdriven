
# Step 1:  execute this to configure debizuim for postgres database
curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" 127.0.0.1:8083/connectors/ --data "@debezium.json"

# step : 2 to test connector plug-ins  are configured 
curl -s http://localhost:8083/connector-plugins | jq

# Step 3 : execute the following tables in posgress databae
CREATE TABLE recipes (
  recipe_id INT NOT NULL,
  recipe_name VARCHAR(30) NOT NULL,
  PRIMARY KEY (recipe_id),
  UNIQUE (recipe_name)
);

CREATE TABLE ingredients (
  ingredient_id INT NOT NULL, 
  ingredient_name VARCHAR(30) NOT NULL,
  ingredient_price INT NOT NULL,
  PRIMARY KEY (ingredient_id),  
  UNIQUE (ingredient_name)
);

ALTER TABLE ingredients REPLICA IDENTITY FULL;
ALTER TABLE recipes REPLICA IDENTITY FULL;

-------------------------------------------

# Step 4 start the consumer for ingreditns table
docker run --tty \
--network rmoff_kafka \
confluentinc/cp-kafkacat \
kafkacat -b kafka:9092 -C \
-s key=s -s value=avro \
-r http://schema-registry:8085 \
-t postgres.public.ingredients

# Step 5 start the consumer for receip table

docker run --tty \
--network rmoff_kafka \
confluentinc/cp-kafkacat \
kafkacat -b kafka:9092 -C \
-s key=s -s value=avro \
-r http://schema-registry:8085 \
-t postgres.public.recipes

# Step 5 start the consumer for receip table

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

 You should see message in the about consumer consule , if works fine. 
