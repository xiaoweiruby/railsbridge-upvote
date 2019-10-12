投票点赞增加和减少，没有限制减少小于的情况，应该给出提示！

config/routes.rb
```
Rails.application.routes.draw do
  resources :topics do
  member do
    post 'upvote'
    post 'downvote'
  end
end
  root 'topics#index'
end
```

app/controllers/topics_controller.rb
```
def upvote
@topic = Topic.find(params[:id])
@topic.votes.create
redirect_to(topics_path)
end

def downvote
@topic = Topic.find(params[:id])
@topic.votes.first.destroy
redirect_to(topics_path)
end
```

app/views/topics/index.html.erb

```
<p id="notice"><%= notice %></p>

<h1>Topics</h1>

<table>
  <thead>
    <tr>
      <th>Title</th>

      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @topics.each do |topic| %>
      <tr>
        <td><%= link_to topic.title, topic %></td>

        <td><%= pluralize(topic.votes.count, "vote") %></td>
        <td><%= button_to '+1', upvote_topic_path(topic), method: :post %></td>
        <td><%= button_to '-1', downvote_topic_path(topic), method: :post %></td>
        <td><%= link_to 'Delete', topic, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Topic', new_topic_path %>
```

https://bbandydd.github.io/blog/2015/05/24/first-rails/
