# README

## Notes

### Adding Model
* Generate model in Command Line - 
  ```bash
  $ bin/rails generate model Comment commenter:string body:text article:references
  ```
* Check model in app/models/comment.rb
* Check migration file
* Migrate - 
  ```bash
  $ bin/rails db:migrate
  ```
* If reference add ASSOCIATION to refrenced (ex. app/models/article.rb) - 
  ```ruby
  class Article < ApplicationRecord
  has_many :comments

  validates :title, presence: true
  validates :body, presence: true, length: { minimum: 10 }
  end
  ```
* Add route. Easy if using resources. (ex. config/routes.rb)
  ```ruby
  Rails.application.routes.draw do
    root "articles#index"

    resources :articles do
      resources :comments
    end
  end
  ```
* Generate a controller in Command Line - 
  ```bash
  $ bin/rails generate controller Comments
  ```

* Wire up controller (ex. app/controllers/comments_controller.rb)
  ```ruby
  class CommentsController < ApplicationController
  def create
    @article = Article.find(params[:article_id])
    @comment = @article.comments.create(comment_params)
    redirect_to article_path(@article)
  end

  private
    def comment_params
      params.require(:comment).permit(:commenter, :body)
    end
end
  ```

* Add to html.erb file