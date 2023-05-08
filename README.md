# OpenAI Cookbook

OpenAI Cookbook은 [OpenAI API]로 일반적인 작업을 수행하기 위한 예제 코드를 공유합니다.

이 예제를 실행하려면 OpenAI 계정과 API 키가 필요합니다([무료 계정 만들기][api 가입]).

대부분의 코드 예제는 Python으로 작성되지만 개념은 모든 언어에 적용될 수 있습니다.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=468576060&machine=basicLinux32gb&location=EastUs)

## Recently added or updated 🆕 ✨

- [Question answering using embeddings](examples/Question_answering_using_embeddings.ipynb) [Apr 14th, 2023]
- [Using vector databases for embeddings search](examples/vector_databases/) [various dates]
- [Powering your products with ChatGPT and your own data](apps/chatbot-kickstarter/powering_your_products_with_chatgpt_and_your_data.ipynb) [Mar 10th, 2023]
- [How to format inputs to ChatGPT models](examples/How_to_format_inputs_to_ChatGPT_models.ipynb) [Mar 1st, 2023]


## Guides & examples

- API usage
  - [How to handle rate limits](examples/How_to_handle_rate_limits.ipynb)
    - [Example parallel processing script that avoids hitting rate limits](examples/api_request_parallel_processor.py)
  - [How to count tokens with tiktoken](examples/How_to_count_tokens_with_tiktoken.ipynb)
  - [How to stream completions](examples/How_to_stream_completions.ipynb)
- ChatGPT
  - [How to format inputs to ChatGPT models](examples/How_to_format_inputs_to_ChatGPT_models.ipynb)
  - [Powering your products with ChatGPT and your own data](apps/chatbot-kickstarter/powering_your_products_with_chatgpt_and_your_data.ipynb)
- GPT
  - [Guide: How to work with large language models](how_to_work_with_large_language_models.md)
  - [Guide: Techniques to improve reliability](techniques_to_improve_reliability.md)
  - [How to use a multi-step prompt to write unit tests](examples/Unit_test_writing_using_a_multi-step_prompt.ipynb)
- Embeddings
  - [Text comparison examples](text_comparison_examples.md)
  - [How to get embeddings](examples/Get_embeddings.ipynb)
  - [Question answering using embeddings](examples/Question_answering_using_embeddings.ipynb)
  - [Using vector databases for embeddings search](examples/vector_databases/Using_vector_databases_for_embeddings_search.ipynb)
  - [Semantic search using embeddings](examples/Semantic_text_search_using_embeddings.ipynb)
  - [Recommendations using embeddings](examples/Recommendation_using_embeddings.ipynb)
  - [Clustering embeddings](examples/Clustering.ipynb)
  - [Visualizing embeddings in 2D](examples/Visualizing_embeddings_in_2D.ipynb) or [3D](examples/Visualizing_embeddings_in_3D.ipynb)
  - [Embedding long texts](examples/Embedding_long_inputs.ipynb)
- Fine-tuning GPT-3
  - [Guide: best practices for fine-tuning GPT-3 to classify text](https://docs.google.com/document/d/1rqj7dkuvl7Byd5KQPUJRxc19BJt8wo0yHNwK84KfU3Q/edit)
  - [Fine-tuned classification](examples/Fine-tuned_classification.ipynb)
- DALL-E
  - [How to generate and edit images with DALL-E](examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb)
- Azure OpenAI (alternative API from Microsoft Azure)
  - [How to use ChatGPT with Azure OpenAI](examples/azure/chat.ipynb)
  - [How to get completions from Azure OpenAI](examples/azure/completions.ipynb)
  - [How to get embeddings from Azure OpenAI](examples/azure/embeddings.ipynb)
  - [How to fine-tune GPT-3 with Azure OpenAI](examples/azure/finetuning.ipynb)
- Apps
  - [File Q and A](apps/file-q-and-a/)
  - [Web Crawl Q and A](apps/web-crawl-q-and-a)

## Related resources

여기에 있는 코드 예제 외에도 다음 리소스에서 [OpenAI API]에 대해 알아볼 수 있습니다.:

- [ChatGPT]로 실험하기
- Try out the API in the [OpenAI Playground]
- [OpenAI 문서]에서 API에 대해 읽어보세요.
- Discuss the API in the [OpenAI Community Forum]
- [OpenAI 도움말 센터]에서 도움을 받으세요.
- See example prompts in the [OpenAI Examples]
- Stay up to date with the [OpenAI Blog]

## Contributing

보고 싶은 예제나 가이드가 있으면 [이슈 페이지]에 자유롭게 제안해 주세요.

[chatgpt]: https://chat.openai.com/
[openai api]: https://openai.com/api/
[api 가입]: https://beta.openai.com/signup
[openai playground]: https://beta.openai.com/playground
[openai 문서]: https://beta.openai.com/docs/introduction
[openai community forum]: https://community.openai.com/top?period=monthly
[openai 도움말 센터]: https://help.openai.com/en/
[openai examples]: https://beta.openai.com/examples
[openai blog]: https://openai.com/blog/
[이슈 페이지]: https://github.com/openai/openai-cookbook/issues
