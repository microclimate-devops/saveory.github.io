---
permalink: /swagger
layout: swagger
consumes:
  - application/json
produces:
  - application/json
title: Saveory API

# tags are used for organizing operations
tags:
- name: Recipes
  description: Operations related to the Recipes Service
- name: Pantry
  description: Operations related to the Pantry Service
- name: User Management
  description: Operations related to the User Management Service
###########################################################
#####################~Login Endpoint~######################
###########################################################
paths:
  /users:
    post:
      tags:
      - User Management
      summary: Signup the user with the received data
      operationId: createUser
      description: Sign the user up and send back the token created for that user
      produces:
      - application/json
      responses:
        200:
          description: Successful Signup. Respond with user_token and message
          schema:
            type: object
        500:
          description: Could not signup the user because the username or email was taken
  /users/login:
    post:
      tags:
      - User Management
      summary: Attempt user login with given username and password
      operationId: loginUser
      description: Check the credentials and respond with the access_token for the user if credentials are correct
      produces:
      - application/json
      responses:
        200:
          description: Successful login. Respond with user_token and message
          schema:
            type: object
        500:
          description: Could not verify login
  /users/{access_token}:
    put:
      tags:
      - User Management
      summary: Attempt to update the user's data
      operationId: updateUser
      description: Updates the database with new information in request's body
      produces:
      - application/json
      parameters:
      - in: path
        name: access_token
        type: string
        required: true
        description: User's access token
      responses:
        200:
          description: Successful update. Respond with user_token and message
          schema:
            type: object
        500:
          description: Could not update the user, check token
    get:
      tags:
      - User Management
      summary: Get the user's data with their access token
      operationId: getUser
      description: Get the user's data from the database
      produces:
      - application/json
      parameters:
      - in: path
        name: access_token
        type: string
        required: true
        description: User's access token
      responses:
        200:
          description: Successful update. Respond with user_token and message
          schema:
            $ref: "#/definitions/UserObj"
        500:
          description: Could not update the user, check token
###########################################################
####################~Recipes Endpoints~####################
###########################################################
  /recipes:
    get:
      tags:
      - Recipes
      summary: User views all recipes
      operationId: recipes
      description: |
        User is able to view all recipes in the application database
      produces:
      - application/json
      parameters:
      - in: header
        name: authorization
        description: "Authorization header containing either \"Basic\" followed by the base 64 encoded form of the string composed of userid and password"
        required: true
        type: string
      - in: query
        name: type
        description: Recipe type
        required: false
        type: string
      - in: query
        name: name
        description: Recipe name
        required: false
        type: string
      responses:
        200:
          description: Successful Request
          schema:
            type: "string"
        500:
          description: Server Error
    post:
      tags:
      - Recipes
      summary: "Admin adds a new recipe"
      description: "The admin can add new recipes into the recipe database"
      operationId: "addRecipe"
      consumes:
      - "application/json"
      parameters:
      - in: header
        name: authorization
        description: "Authorization header containing either \"Basic\" followed by the base 64 encoded form of the string composed of userid and password"
        required: true
        type: string
      - in: "body"
        name: "body"
        description: "Recipe object that will be added to the store"
        required: true
        schema:
          $ref: "#/definitions/Recipe"
      responses:
        201:
          description: "Succesfully added"
        405:
          description: "Invalid input"
  /recipes/{recipeId}:
    get:
      tags:
      - Recipes
      summary: User views detailed information of a recipe
      operationId: getRecipeById
      description: |
        User is able to view the details of the selected recipe
      produces:
      - application/json
      parameters:
      - in: header
        name: authorization
        description: "Authorization header containing either \"Basic\" followed by the base 64 encoded form of the string composed of userid and password"
        required: true
        type: string
      - name: "recipeId"
        in: "path"
        description: "ID of receipe to return"
        required: true
        type: "integer"
        format: "int64"
      responses:
        200:
          description: Successful Request
          schema:
            type: "string"
        500:
          description: Server Error
    put:
      tags:
      - Recipes
      summary: "Admin modifies a previously added recipe"
      description: "The admin can modify the information of a previously added recipe"
      operationId: "modifyRecipe"
      consumes:
      - "application/json"
      parameters:
      - in: header
        name: authorization
        description: "Authorization header containing either \"Basic\" followed by the base 64 encoded form of the string composed of userid and password"
        required: true
        type: string
      - in: "body"
        name: "body"
        description: "Recipe object with according modifications"
        required: true
        schema:
          $ref: "#/definitions/Ingredient"
      responses:
        201:
          description: "Succesfully modified"
        405:
          description: "Invalid input"
    delete:
      tags:
      - Recipes
      summary: Admin removes a recipe
      description: |
        Admin can remove a recipe of the recipe database
      operationId: "removeRecipe"
      parameters:
      - in: header
        name: authorization
        description: "Authorization header containing either \"Basic\" followed by the base 64 encoded form of the string composed of userid and password"
        required: true
        type: string
      - name: "recipeId"
        in: "path"
        description: "ID of recipe to return"
        required: true
        type: "integer"
        format: "int64"
      responses:
        200:
          description: Successfully Removed
          schema:
            type: "string"
        500:
          description: Server Error
  /recipes/favorites:
    get:
      tags:
      - Recipes
      summary: User views all his favorite recipes
      operationId: getFavoriteRecipes
      description: |
        User is able to view his/her favorite recipes
      parameters:
      - in: header
        name: authorization
        description: "Authorization header containing either \"Basic\" followed by the base 64 encoded form of the string composed of userid and password"
        required: true
        type: string
      produces:
      - application/json
      responses:
        200:
          description: Successful Request
        500:
          description: Server Error
  /recipes/available:
    get:
      tags:
      - Recipes
      summary: User views all currently cookable recipes
      operationId: getAvailableRecipes
      description: |
        User is able to view all cookable recipes with the current ingredients
      parameters:
      - in: header
        name: authorization
        description: "Authorization header containing either \"Basic\" followed by the base 64 encoded form of the string composed of userid and password"
        required: true
        type: string
      produces:
      - application/json
      responses:
        200:
          description: Successful Request
        500:
          description: Server Error

###########################################################
####################~Pantry Endpoints~#####################
###########################################################
  /pantry/{access_token}:
    get:
      tags:
        - Pantry
      summary: Get the pantry of the current user
      operationId: getPantry
      description: User is able to view all the ingredients currently in his/her pantry
      produces:
        - application/json
      parameters:
        - in: path
          name: access_token
          type: string
          required: true
          description: User's access token
      responses:
        200:
          description: Respond with user's apntry
          schema:
            $ref: "#/definitions/Pantry"
        500:
          description: Server Error
    delete:
      tags:
      - Pantry
      summary: Delete user's pantry
      description: Delete pantry from the database
      operationId: deletePantry
      consumes:
      - "application/json"
      parameters:
        - in: path
          name: access_token
          type: string
          required: true
          description: User's access token
      responses:
        200:
          description: Respond with code to indicate success or failure and message
          schema:
            type: object
        500:
          description: Server Error
  /pantry/{access_token}/ingredient:
    post:
      tags:
      - Pantry
      summary: Add ingredient to user's pantry
      description: Update the user's pantry in the database with the new ingredient
      operationId: addIngredient
      consumes:
      - "application/json"
      parameters:
        - in: path
          name: access_token
          type: string
          required: true
          description: User's access token
        - in: "body"
          name: "body"
          description: Ingredient object
          required: true
          schema:
            $ref: "#/definitions/Ingredient"
      responses:
        200:
          description: Respond with code to indicate success or failure and message
          schema:
            type: object
        500:
          description: Server Error
  /pantry/{access_token}/ingredients:
    get:
      tags:
      - Pantry
      summary: Get list of ingredient names in the user's pantry
      description: Get the user's pantry, from the token, and process all ingredients to make a list of names
      operationId: getIngredientNames
      consumes:
      - "application/json"
      parameters:
        - in: path
          name: access_token
          type: string
          required: true
          description: User's access token
      responses:
        200:
          description: Respond with code to indicate success or failure and message
          schema:
            type: object
        500:
          description: Server Error
  /pantry/{access_token}/ingredients/auto:
    put:
      tags:
      - Pantry
      summary: Attempts to update ingredients in the user's pantry automatically
      description: Update the user's pantry with ingredient quantity usage, attempts to convert units automatically
      operationId: updateIngredientsAutomatically
      consumes:
      - "application/json"
      parameters:
        - in: path
          name: access_token
          type: string
          required: true
          description: User's access token
        - in: "body"
          name: "body"
          description: Ingredient update list
          required: true
          schema:
            $ref: "#/definitions/IngredientUpdateList"
      responses:
        200:
          description: All ingredients were updated automatically
          schema:
            type: object
        206:
          description: Some ingredients failed automatic unit conversion and require user assistance
          schema:
            type: object
        500:
          description: Server Error
  /pantry/{access_token}/ingredients/manual:
    put:
      tags:
      - Pantry
      summary: Updates ingredients in the user's pantry manually
      description: Update the user's pantry with new ingredient quantity and unit data
      operationId: updateIngredientsManually
      consumes:
      - "application/json"
      parameters:
        - in: path
          name: access_token
          type: string
          required: true
          description: User's access token
        - in: "body"
          name: "body"
          description: Ingredient update list
          required: true
          schema:
            $ref: "#/definitions/IngredientUpdateList"
      responses:
        200:
          description: Ingredients were updated
          schema:
            type: object
        500:
          description: Server Error
  /pantry/{access_token}/ingredient/{ingredient_id}:
    put:
      tags:
      - Pantry
      summary: Update the ingredient with new information
      operationId: updateIngredient
      description: Update the ingredient in the user's pantry that matches the ID inside the database
      produces:
      - application/json
      parameters:
        - in: path
          name: access_token
          type: string
          required: true
          description: User's access token
        - in: path
          name: ingredient_id
          type: string
          required: true
          description: The ID of the ingredient to update
        - in: "body"
          name: "body"
          description: Ingredient object
          required: true
          schema:
            $ref: "#/definitions/Ingredient"
      responses:
        200:
          description: Respond with code to indicate success or failure and message
          schema:
            type: object
        500:
          description: Server Error
    delete:
      tags:
      - Pantry
      summary: Delete the ingredient from the user's pantry
      description: Remove the ingredient in the user's pantry inside the database
      operationId: "removeIngredient"
      parameters:
        - in: path
          name: access_token
          type: string
          required: true
          description: User's access token
        - in: path
          name: ingredient_id
          type: string
          required: true
          description: The ID of the ingredient to update
      responses:
        200:
          description: Respond with code to indicate success or failure and message
          schema:
            type: object
        500:
          description: Server Error
  /pantry/spec/ingredient:
    get:
      tags:
      - Pantry
      summary: Gets list of ingredient fields
      description: An array of all the fields of the ingredient object is sent
      operationId: getIngredientFields
      consumes:
      - "application/json"
      responses:
        200:
          description: List of ingredient fields
          schema:
            type: array
            items:
              type: string
              example: ["ingredient", "unit", "quantity"]
        500:
          description: Server Error
  /pantry/spec/ingredient/id:
    get:
      tags:
      - Pantry
      summary: Gets the ingredient field that should be used to identify each ingredient
      operationId: getIngredientId
      consumes:
      - "application/json"
      responses:
        200:
          description: ingredient id field
          schema:
            type: string
        500:
          description: Server Error
  /pantry/spec/ingredient/types:
    get:
      tags:
      - Pantry
      summary: Gets list of ingredient field types
      description: An array of all the field types of the ingredient object is sent
      operationId: getIngredientTypes
      consumes:
      - "application/json"
      responses:
        200:
          description: List of ingredient field types
          schema:
            type: array
            items:
              type: string
              example: ["text", "number", "date", "{\"location\":\"text\"}"]
        500:
          description: Server Error
  /pantry/spec/ingredient/units:
    get:
      tags:
      - Pantry
      summary: Gets list of culinary units that can be processed by the api
      operationId: getIngredientUnits
      consumes:
      - "application/json"
      responses:
        200:
          description: List of culinary units
          examples:
            application/json: >
              ["cup","tsp","tbsp","oz","lbs"]
        500:
          description: Server Error
  /pantry/spec/ingredient/edits:
    get:
      tags:
      - Pantry
      summary: Gets list of boolean values that represent the ability to edit each ingredient field
      description: An array of all the field editable info of the ingredient object is sent
      operationId: getIngredientFieldEdits
      consumes:
      - "application/json"
      responses:
        200:
          description: List of ingredient field edits
          schema:
            type: array
            items:
              type: boolean
          examples:
            application/json: >
              [true, false]
        500:
          description: Server Error
  /pantry/spec/ingredient/location:
    get:
      tags:
      - Pantry
      summary: Gets valid options for location field
      operationId: getIngredientLocationOptions
      consumes:
      - "application/json"
      responses:
        200:
          description: valid options for location field
          schema:
            type: array
            items:
              type: string
              example: ["Refrigerator", "Pantry"]
        500:
          description: Server Error

###########################################################
#####################~Object Definitions~##################
###########################################################

definitions:
  UserObj:
    type: object
    required:
    - username
    - name
    - email
    - token
    properties:
      username:
        type: string
        example: "larry"
      name:
        type: string
        example: "larry G"
      email:
        type: string
        example: "larry@test.org"
      token:
        type: string
        example: "7vfvsdfsdf90932"
  Pantry:
    type: object
    required:
    - id
    - user
    - pantry
    properties:
      id:
        type: string
        example: "3f4tfsd434"
      user:
        type: string
        example: "4q4zxcpd"
      pantry:
        type: array
        items:
          $ref: "#/definitions/Ingredient"

  Ingredient:
    type: object
    required:
    - ingredient
    - quantity
    - unit
    - expiration
    - location
    properties:
      ingredient:
        type: string
        example: "Milk"
      quantity:
        type: number
        format: double
        example: 4
      unit:
        type: string
        example: "cup"
      expiration:
        type: string
        format: date
        example: 02/30/16
      location:
        type: string
        example: "Refrigerator"
  IngredientUpdateList:
    type: array
    items:
      $ref: "#/definitions/IngredientUpdate"
  IngredientUpdate:
    type: object
    required:
    - ingredient
    - quantity
    - unit
    properties:
      ingredient:
        type: string
        example: "Milk"
      quantity:
        type: number
        format: double
        example: 4
      unit:
        type: string
        example: "cup"
  Recipe:
    type: object
    required:
    - name
    - id
    - description
    - instructions
    - ingredients
    - tag
    properties:
      name:
        type: string
        example: "Cereal and Milk"
      id:
        type: string
        example: "3f920f90wu9f0ds"
      description:
        type: string
        example: "Simple and tasty recipe of milk and cereal!"
      instructions:
        type: string
        example: "1. First we need to.... 2. Then we..."
      ingredients:
        type: array
        #xml:
        #name: "Ingredient"
          #wrapped: false
          #not sure what "wrapped" means in this context
        items:
          $ref: "#/definitions/RecipeIngredient"
        #Verify how to correctly define as array
      tag:
        type: array
        items:
          type: string
          example: ["grains", "dairy"]
  RecipeIngredient:
    type: object
    required:
    - name
    - id
    - quantity
    - notes
    properties:
      name:
        type: string
        example: "Milk"
      id:
        type: string
        example: "90jf90j43few0fjoij"
      quantity:
        type: number
        format: double
        example: 4
      notes:
        type: string
        example: Kellog's

# Added by API Auto Mocking Plugin
host: virtserver.swaggerhub.com
basePath: /Ykram12/Pantry/1.0.0
schemes:
 - https
---
