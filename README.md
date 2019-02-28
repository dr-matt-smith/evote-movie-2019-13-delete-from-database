# evote-movie-2019-13-delete-from-database


Part of the progressive Movie Voting website project at:
https://github.com/dr-matt-smith/evote-movie-2019

The project has been refactored as follows:

- add a delete movie method admin controller `/src/AdminController.php`:

    ```php
          public function deleteMovie()
          {
              $id = filter_input(INPUT_GET, 'id');
      
              $movieRepository = new MovieRepository();
              $success = $movieRepository->delete($id);
      
              if($success){
                  // now list all movies
                  $mainController = new MainController();
                  $mainController->listMovies();
              } else {
                  $errors = [];
                  $errors[] = "there was an error trying to DELETE movie with id = '$id''";
                  $pageTitle = 'error';
                  require_once __DIR__ . '/../templates/error.php';
              }
          }
    ```

- add a new route for `action=deleteMovie` to the Front Controller `/public/index.php` to invoke the `deleteMovie()` method of an admin controller:

    ```php
          // ------ admin section --------
          case 'newMovieForm':
              $adminController->newMovieForm();
              break;
      
          case 'createNewMovie':
              $adminController->createNewMovie();
              break;
      
          case 'deleteMovie':
              $adminController->deleteMovie();
              break;
    ```

- update the movie list template `/templates/list.php` to add an extra column, offering a `DELETE` link, that passes `action=deleteMovie` and also passes the `id` of the movie being displayed by the loop, this involves adding `id=` followed by the `id` of the current `Movie` object in the loop:

    ```php
        <table>
            <tr>
                <th> ID </th>
                <th> title </th>
                <th> price </th>
                <th> &nbsp; </th>
            </tr>
        
            <?php
                foreach($movies as $movie):
            ?>
                <tr>
                    <td><?= $movie->getId() ?></td>
                    <td><?= $movie->getTitle() ?></td>
                    <td>&euro; <?= $movie->getPrice() ?></td>
                    <td>
                        <a href="index.php?action=deleteMovie&id=<?= $movie->getId() ?>">DELETE</a>
                    </td>
                </tr>
    ```
    
    - so each movie now has a `DELETE` link in the form `index.php?action=deleteMovie&id=3` (or 4 or whatever the `id` value is)
    
