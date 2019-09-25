===============
Source Code
===============

model 1
^^^^^^^^^^^^^^

.. code-block:: python

    image = Input(shape=(block_size,block_size,NUM_CHANNELS))

    image_norm = Lambda(sub_mean)(image)

    conv1 = Conv2D(16, (3, 3), strides =(1,1),padding='valid', activation='relu')(image_norm)
    
    conv2 = Conv2D(32, (3, 3), strides =(1,1), padding='valid', activation='relu')(conv1)
   
    conv2_pooling = MaxPooling2D(pool_size=(2, 2))(conv2)
    conv2_drop = Dropout(rate = 0.5)(conv2_pooling)

    flat2 = Flatten()(conv2_drop)

    qp = Input(shape=(1,))

    qp_n = Lambda(lambda x: x/255)(qp)

    flat2_qp = Concatenate(axis=1)([flat2, qp_n])

    fc1 = Dense(128, activation='relu')(flat2_qp)
    fc1_d = Dropout(rate = 0.5)(fc1)

    fc1_qp = Concatenate(axis=1)([fc1_d, qp_n])

    output = Dense(num_classes, activation='softmax')(fc1_qp)

    model = Model(inputs=[image,qp], outputs=output)

    model.summary()

    model.compile(loss='categorical_crossentropy',
                  optimizer=keras.optimizers.Adam(lr=0.001),
                  metrics=['accuracy'])


model 2
^^^^^^^^^^^^^^

.. code-block:: python

 data = Input(shape=(block_size,block_size,NUM_CHANNELS))

    data_norm = Lambda(sub_mean)(data)

    conv1 = Conv2D(32, (3, 3), strides =(1,1),padding='valid', activation='relu')(data_norm)
    conv1_dropout = Dropout(rate=0.5)(conv1)
 

    conv2 = Conv2D(64, (3, 3), strides =(1,1), activation='relu', padding='valid')(conv1)
    conv2_dropout = Dropout(rate=0.5)(conv2)
    
    conv3 = Conv2D(128, (3, 3), strides =(1,1), activation='relu', padding='valid')(conv2)
    conv3_dropout = Dropout(rate=0.5)(conv3)

    conv3_pooling = MaxPooling2D(pool_size=(2, 2))(conv3_dropout)
    conv3_d = Dropout(0.25)(conv3_pooling)
    flat3 = Flatten()(conv3_d)

    qp = Input(shape=(1,))
    qp_n = Lambda(lambda x: x/255)(qp)

    concat = Concatenate(axis=1)([flat3, qp_n])

    fc1 = Dense(128, activation='relu')(concat)
    fc1_d = Dropout(rate=0.5)(fc1)
    fc1_qp = Concatenate(axis=1)([fc1_d, qp_n])

    #fc2 = Dense(48, activation='relu')(fc1_qp)
    #fc2_qp = Concatenate(axis=1)([fc2, qp_n])

    output = Dense(num_classes, activation='softmax')(fc1_qp)

    model = Model(inputs=[data,qp], outputs=output)

    model.summary()

    model.compile(loss='categorical_crossentropy',
                  optimizer=keras.optimizers.Adam(lr=0.001),
                  metrics=['accuracy'])
