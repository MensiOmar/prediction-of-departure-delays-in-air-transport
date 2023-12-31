# prediction-of-departure-delays-in-air-transport
Personal Contribution of the
Internship
- Key learnings achieved
    - Project description
The aim of this internship project is to develop a predictive model for transportation delays using deep learning methods. Transportation delays can greatly
impact passenger satisfaction and the efficiency of transportation operations. By
utilizing historical data and harnessing the powerful modeling capabilities of deep
learning, the developed model will be able to predict potential delays with enhanced
accuracy. This project consists of 4 main tasks which are listed as follows :
1. Data Collection and Preprocessing
2. Designing the Model Architecture
3. Entraînement du modèle
4. Validation et évaluation du modèle

    1. Data Collection and Preprocessing
For the Data Collection and Preparation stage, I utilized the "2015 Flight
Delays and Cancellations" dataset from Kaggle. This dataset is sourced from the
U.S. Department of Transportation’s (DOT) Bureau of Transportation Statistics,
which tracks the performance of domestic flights operated by major air carriers. It
provides valuable information regarding on-time, delayed, canceled, and diverted
flights. The dataset was chosen due to its relevance and comprehensiveness, as it
aligns with the objectives of this project.
The context of the dataset is centered around the monthly Air Travel Consumer
Report published by the DOT, which offers an overview of flight performance. The
dataset’s information is integral to understanding flight delays and cancellations
during the year 2015.
In addition, I drew insights from a scientific article titled "A Multi-Step Airport
Delay Prediction Model Based on Spatial-Temporal Correlation and Auxiliary
Features" authored by Hao Zhang, Chunyue Song, Jie Zhang, Hui Wang, and
Jinlong Guo. This article, published on 08 May 2021, provided valuable guidance
for preprocessing the dataset according to the article’s requirements. By leveraging
the scientific insights presented in this article, I refined and prepared the dataset,
ensuring its alignment with advanced delay prediction methodologies.
During the data processing phase, a crucial yet time-consuming task was undertaken. Initially, I focused on refining the time-related information provided in the
dataset. This information was distributed across separate columns denoting the
year, month, day, and departure time. To transform the problem into a time series
context, I employed the Python datetime library along with algorithmic techniques.
This process involved creating a unified timestamp by uniting the scattered time
components.
It is noteworthy that the time information furnished in the dataset corresponds
to the local departure time of the respective airports. Given the multi-time zone
nature of the United States, accounting for time zones was pivotal. To address this,
I precisely collected the time zone data for each of the 322 airports in the United
States. This endeavor ensured accurate time synchronization across the dataset.
Central to this effort was the conversion of all timestamps to the UTC time zone,
which served as a unifying reference point.

Dealing with the time-related problems demanded an complex yet essential
effort. The resulting time series data, harmonized across diverse airport locations,
facilitated the creation of a comprehensive and reliable framework for subsequent
analyses and modeling.
After a thorough examination of the article, I attentively began to prepare the
inputs for the model. First of all, the article focused on departure delay forecasting.
Consequently, I prepared the delay feature for each flight according to the proposed
formula:
dep_delay = max(0,real_dep_time − expected_dep_time − 15)
It is important to mention that some missing values were encountered in relation
to this information, resulting in the removal of these corresponding rows. Additionally, I excluded noisy data with delays exceeding 6 hours.Due to the difference of
magnitude and distribution of airports, it is necessary to normalise the data. The
max–min normalisation method is used to scale the data at [0,1]
Subsequent to this, I generated an entirely new dataframe to accommodate the
input data. This dataframe encompassed the average airport delay for each hour.
With 322 columns representing distinct airports and a time column indicating the
date and time of the specific hour, the dataframe was structured to mirror the time
series format advocated by the article. Consequently, the dataframe contained 8037
rows, each corresponding to an hour and its associated average airport delays.

The article treated airports as nodes and delays as edge cost values in a directed
graph, representing the interconnectedness of airports and the delay status at each
hour. Building upon the previously crafted dataset, an adjacency matrix was
created to illustrate the network of connections among airports at each hour. This
adjacency matrix serves as input to the Spatial Correlation Extraction module,
which employs the PageRank algorithm. The module generates a vector, encompassing the significance of each airport within that network at a specific time. This
vector is subsequently multiplied with the actual delay vector for the same time,
enhancing its value. The resulting enriched data, along with auxiliary features, is
then fed into the model as input.

By applying this approach, the article’s methodology leverages network analysis
techniques to extract crucial spatial correlations between airports and their delay
patterns. The resulting data not only encapsulates the inherent relationships within
the airport network but also incorporates them as valuable input features for the
predictive model. This innovative strategy contributes to a more comprehensive
understanding of the underlying factors influencing departure delays and enhances
the model’s predictive capabilities.

    2. Designing the Model Architecture
        - Multi-Step Prediction Approach
This section introduces the LSTM-Seq2Seq model, designed to handle time series
prediction tasks. Unlike traditional models, LSTM excels in capturing long-term
patterns due to its memory unit. The model features three gates: forget, input,
and output. These components collectively manage the storage and utilization of
short and long-term information. A detailed illustration of the LSTM structure is
provided in Figure 2, with further details in the associated caption.

The Seq2Seq model enhances multi-step predictions by reducing error accumulation issues. It is especially useful when input and output sequence lengths
vary. This combination of models has shown success in machine translation and
transportation prediction tasks. To overcome challenges posed by accumulated
errors in multi-step prediction, the Seq2Seq model has been adopted.
Seq2Seq involves an encoder and decoder. The encoder processes input sequences and produces a context vector. The decoder generates predictions using
the context vector and its input.
To address extended predictions and reduce error buildup, a temporal attention
mechanism is introduced. This mechanism improves model interpretability by
measuring the relevance between encoder and decoder hidden states. By assessing
similarities between states, it enhances the prediction accuracy.
In the final model, the weighted hidden state, original hidden state, and auxiliary
features are combined in a fully connected layer to produce final delay prediction
sequences for airports.
    
    - Simpler Model Exploration
While this model inspired my work, its complexity and time constraints led
me to pursue a simpler approach. My smaller models consisted of input layers
experimented with LSTM, GRU, and Conv1D layers. These were followed by a
fully connected layer and an output layer. I conducted numerous experiments
by tweaking parameters. To transform the time series problem into a supervised
learning setup, I employed windowing algorithms. This allowed me to generate
inputs for the model. Although I couldn’t fully realize the elaborate model due
to its complexity and time limitations, I was able to craft alternative models that
produced promising results, catering to the constraints at hand.

 3. Training the models
In this subsection, the training process of the models is elaborated. The experiments were conducted using an Intel Core i5 11th generation processor and a GTX
1650 graphics card with TensorFlow and the Keras API.
Two distinct experiments were carried out:
1. Single-Feature Experiment: This experiment involved training a model
using only the departure delay as the sole input feature.
2. Feature-Enriched Experiment: In this case, auxiliary features were incorporated to enhance predictions. These auxiliary features included sine and
cosine representations of day and month to capture the periodicity associated
with these time-related factors.

Both experiments followed the same methodology, adhering to the article’s
recommendation of employing a windowing algorithm with a window size of 7 hours
for each target prediction. Three different model architectures were utilized: Long
Short-Term Memory (LSTM), Convolutional 1D (Conv1D), and Gated Recurrent
Unit (GRU). These models were compared based on their performance.
To ensure a robust evaluation, the dataset was divided as follows:
• Training Dataset: 70 percent of the data was allocated for training purposes.
• Validation Dataset: 10 percent of the data served as a validation set.
• Test Dataset: The remaining 20 percent of the data was reserved for testing
the models’ generalization capabilities.
By adhering to these division ratios, the models’ performance could be accurately
assessed and compared across different architectures and experiments

  4.Validation and model evaluation
During the validation and evaluation phase, the Mean Squared Error (MSE)
values were computed as indicators of model performance. Across all experiments,
the MSE values consistently fell within the range of 0.0020 to 0.0024, suggesting a
consistent level of accuracy in capturing departure delay patterns.
In the first experiment, the models exhibited varying degrees of performance in
the validation phase. While the LSTM and GRU models struggled to closely follow
the observed delay patterns, the Conv1D model displayed a noteworthy ability
to capture delay trends. However, the Conv1D model was not impervious to the
occurrence of sudden high peaks.
In the second experiment, where auxiliary features were integrated, all three
models demonstrated the ability to capture delay patterns to a greater extent in
the validation plots. The LSTM, Conv1D, and GRU models showcased improved
performance in this context. Particularly, the GRU model stood out, exhibiting
the most robust performance in detecting delay patterns, attaining an MSE of 0.002.
To illustrate these outcomes, validation plots were generated for the bestperforming model from each experiment. These plots visually depict how well
the models’ predictions aligned with actual observed delays. The MSE values, in
conjunction with the validation plots, provide a comprehensive understanding of the
models’ predictive capabilities in simulating departure delay trends. These results

serve as a foundation for selecting optimal models for deployment and inform the
potential improvements that can be made for enhanced real-world performance.