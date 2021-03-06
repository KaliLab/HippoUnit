#####
Tests
#####

Implementation of the Tests
------------------------------------------------------

Each test of HippoUnit is a separate Python class that inherits from ``SciUnit.Test`` class. The test class must accept the *observation* argument, which is a Python dictionary containing the reference experimental data for the test. It must implement the ``generate_prediction()`` method to generate the model's response (the *prediction*), and the ``compute_score()`` method, which, after comparing the *prediction* to the *observation*, returns the final score achieved by the model on the given test. The test class must specify the *required_capabilities* and must define the *score_type* to be used. To run a test and get the score, the test’s ``judge()`` function is called, which is inherited from ``SciUnit.Test`` class and not implemented in HippoUnit itself. This checks the availability of the *capabilities*, calls the ``generate_prediction()`` method and then the ``compute_score()`` method to return the final score. 

The following arguments are accepted by each of the tests: *observation*, *config*, *force_run*, *base_directory*, *show_plot*, *save_all*. The *observation* argument is a Python dictionary (loaded from a JSON file) that contains the features evaluated by the test as keys and the experimental mean and standard deviation of each feature, typically calculated from results of measurements in several different cells. The *config* argument is also a dictionary through which the parameters of the simulation protocols are set in order to be in correspondence with the experimental protocol from which the *observation* data derive. (The Oblique Integration Test and the Depolarization Block Test do not have this argument because, in these cases, the protocols are hard-coded.)

After running the simulations all of the tests save the measured response of the model (e.g., voltage traces) into pickle files. If a specific test is run again on a specific model and the pickle files already exist, the actions performed by the test depend on the value of the Boolean argument ``force_run``. If this argument is set to False, the simulations are not run again on the model, and the data from the pickle files are used for feature extraction. This is useful if one wants to test a model with the same test but against different observation data. If the ``show_plot`` boolean argument is set to False, the output figures are not displayed, but they are still saved. When the ``save_all`` boolean variable is set to False, only the JSON files containing the absolute feature values, the feature error scores and the final scores, and a log file are saved, but the figures and pickle files are not.

The ``base_directory`` is a string which gives the path where the outputs of the tests are saved. Both the ``ModelLoader()`` class and the test classes have this argument (with the default value ‘None’). If the user sets the base directory of the test class, the outputs will be saved in the location defined by the following path structure: *test.base_directory + 'temp_data or figs or results /' + 'name of the test/' + model.name + '/'*. In this case, if a test is applied to multiple models, all the results can be saved in the same directory. On the other hand, if the user sets the base directory of the ModelLoader class, the outputs will be saved using a path of the following structure: *model.base_directory + 'temp_data or figs or results /' + 'name of the test/'*. This provides the opportunity to save all the validation results of multiple tests of the same model in the directory of the model.


.. automodapi:: hippounit.tests
    :nosignatures:
    :no-main-docstr:
    :skip: {{ test_classes|join(', ') }}